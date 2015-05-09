The GitTorrent Protocol (GTP) is a protocol for collaborative [git](http://git.or.cz/) repository distribution across the Internet.

It might currently come across as a solution looking for a problem - and as one smart-ass with admin rights to the Google Code project reminds you on the source tab, "more alpha than the greek letter".  The initial motivation was performance of downloads and in particular reducing load on kernel.org.

That's one reason d'etre, but to those who argue that is insufficient justification for its existence, that Git is already fast enough - it is a first step towards applying decentralizing Peer to Peer concepts to Git.  If you decentralize the download layer, it's just another small step before you decentralize the push rights and tie it to a web of trust such as PGP, and then you don't actually need discrete mirror sites.  Every mirror can track the git repositories the owners want it to carry, and those authorized to sign updates can make signed updates to push the cloud forward.  Your local mirror can become a one-stop git push and pull stop depot, and the source code is preserved in many more places, increasing resilience, availability and download performance for all.

These longer-term plans would have been included in the [RFC](http://gittorrent.utsl.gen.nz/rfc.html), but for a desire to keep it simple and build the first layer first.

## Status and History ##

Last status announcement was at [August 2008](http://lists.utsl.gen.nz/pipermail/gittorrent/2008-August/000047.html).  That demonstrated a program which started two gittorrent daemons, which connect to each other over TCP and exchange a repository using the messages in the RFC.

So, initially, Jonas came up with the idea and made a thesis out of it.  Along the way it was I think mentioned on #git, and I (Sam) got involved, reviewed the design and came up with a design that did not require unnecessary extra indexes to be distributed among peers, while still making good use of delta compression.  ie, when pulling over a limited bandwidth connection there should be minimal extra downloaded compared to pulling from one server.

So, we've been through 2 rounds of Google Summer of Code participation, and a small amount of time from me (Sam) in getting it this far.  Currently no-one is (to my knowledge) actively developing either this developed version (sorry, I don't get paid for this, it's really just when I have the time, inclination etc) or Jonas' C++ implementation.

What's likely to happen next is that at the [GitTogether](http://git.or.cz/gitwiki/GitTogether), this will be explained and a simple path forward laid that will layer this technology over git's native protocol, and leave the BitTorrent baggage behind.  If there is sufficient interest, then the project may get another breath of life.

## Welcome Slashdotters ##

It has been pointed out that there has been no update on what happened at GitTogether.  Basically, as the final paragraph in the above points out, it needs to work over the git:// protocol to be useful, and as it turns out the design of git makes this much simpler than I originally thought.  A proof of concept could in principle be thrown together as a single script loosely based on the existing Perl implementation, and a trivial patch to the git-daemon to enable it.  I've put the sketch of this on this wiki using the name it will probably end up with - "MirrorSync"

Prototypes for the individual messages are still pending, but hopefully you can see that the design is now sufficiently simple that this shouldn't be an arduous task... take a look and please comment if you've something to add!