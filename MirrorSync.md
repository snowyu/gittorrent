# Introduction #

This protocol was designed to be a stripped-down version of the GitTorrent protocol, instead of basing its core design on the binary BitTorrent protocol it is designed to fit cleanly into git.  Some of the core concepts - in particular, the design for dividing pushed and pulled packs into roughly equal sections - will be carried over.  But it will look very different to BitTorrent by the end of this.

This page is essentially copying all of the details from
http://sam.vilain.net/talks/gittorrent/slide10a.html, but with explanatory notes which weren't in the original slideshow (which, of course, I explained in person!)

# Key Mirror Sync Protocol Messages #

The entire Torrent process was broken down into these three parts.  In git development fashion each of which are to be evaluated and implemented independently and each one incrementally adds benefits.  The result is hopefully something with all the features of BitTorrent and none of teh suk.

  * Mirror List - "get me a list of current mirrors, and tell me the latest 'ref'"
  * Mirror Notify - "FYI, I'm mirroring repository X at location Y.  HAND!"
  * Mirror Sync - "Oh hai, can you hand me a slice of git pack?  And want any of mine?"

## Mirror List ##

This request replaces the Tracker HTTP protocol.

Example request (for brevity, the vhost part of the request has not been included)

```
mirror-list /pub/scm/linux/kernel/git/torvalds/linux-2.6.git
```

The response to this might be;

```
auth refs/* 82312e41472b5f8d86e36e30fc06408866b25843
latest f1d2d2f924e986ac86fdf7b36c94bcdf32beec15
try git://repo.or.cz/linux-2.6 86400 current
try http://git.utsl.gen.nz/mirror/repo.or.cz/linux-2.6 86400 current
try git://214.23.35.210/ 3600 partial
```

To disect and explain that;

  * The "latest" part is the SHA1 of the current packed-refs file which represents the result of the most current push to this repository; to be useful, we also need to be able to retrieve the packed-refs file in question.  Currently this was designed to be encapsulated in a git tag object, to allow for comments and PGP signatures without more infrastructure.
  * The `auth refs/* 823..66b25843` part is saying that "if you see a packed-refs/tag object signed by key ID 66b25843, and it changes references matching refs/, it's valid." - there's a slight security flaw with issuing authorization like this over an unauthenticated channel; my current thoughts are that the web of trust and saving known keys like ssh does will make this not a real issue.
  * All of the "try" lines are mirrors; the master server may or may not have already confirmed that they work.  The times after the URL represent the TTL of this mirror line, and the final word represents whether or not when it was last checked that this mirror was up to date or not.  That last one, for instance, is more BitTorrent-style; it's a peer which has only partially downloaded the repository and didn't even supply a DNS name for itself.

## Mirror Notify ##

This is broken into a separate message, so that the existing git facility to enable and disable `git-daemon` commands can be used without specific configuration.

```
mirror-notify /pub/scm/linux/kernel/git/torvalds/linux-2.6.git
at git://214.23.35.210/linux-2.6.git
latest f1d2d2f924e986ac86fdf7b36c94bcdf32beec15
```

Response:

```
ok
```

It's notifying the server that you're accepting connections from people for downloads.  Unlike with BitTorrent, it doesn't require that everyone at least try to serve content - something that those on the edges of the 'net would quite often find frustrating; especially when the tables of peers would be filled with private addresses and filtered endpoints.

I'd like to make the default implementation of this "mirror-notify" command at least try to call back the URL passed, to see if there is indeed a repository sitting there, and perhaps check it to see whether it's current or not.

## Mirror Sync ##

This command is all about making the job of pushing and pulling asymmetric, as well as much more efficient; basically there should be no creating of packs for synchronisation to occur.  It is the most complicated message, but still I'm aiming for an initial implementation in a Perl script which should be digestable in a single reading.

For this message, I've just marked the lines received from the end making the call with `<=` and those sent with `=>`

```
mirror-sync /pub/scm/linux/kernel/git/torvalds/linux-2.6.git
<= mirror-sync ready
<= have push f1d2d2f9 auth 66b25843
=> fetch push f1d2d2f9
<= receive push f1d2d2f9 size 1234
...1234 bytes...
=> want bundle :f1d2d2f9
<= have bundle :ba023123 20MB 1
<= have bundle ba023123:f1d2d2f9 2MB 0101
=> fetch bundle :ba023123 part 17/32
<= receive bundle :ba023123 part 17/32 size 732412
```

To read this - and remember it is really only a sketch - first there is an exchange of "push" messages; these are the tags containing packed-refs files, which the GTP protocol describes as a "References" file.  The peers first exchange these objects and decide which ones are the most current.

They'll then exchange information about what bundles they have available - a bundle being a pack containing all of the new objects between two sets of pushes.  These are actually already defined git concepts.  There is information about how big the bundle is in total, and the peer supplies their bitmap of which slices of the bundle they have.  The slicing algorithm is defined in the GTP protocol.

Once the ends have decided what they want out of this connection, they'll start swapping bundle fragments, and after a fragment is swapped they might also notify the peer of any new push objects they have, or update the other end with any changed bitmaps of bundle fragments.