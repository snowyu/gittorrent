To date, two different protocol specification have been proposed.

## Initial Protocol Draft ##

The initial RFC draft was the result of a [project](http://gittorrent.googlecode.com/files/Fonseca-2006.pdf) done at the Department of Computer Science, University of Copenhagen ([DIKU](http://www.diku.dk)) in 2006.

The protocol proposed in the initial RFC draft tried to reuse as much as possible from git's existing protocols in the exchange of the repository database. The result was a protocol that basically worked somewhat similar to git's "dumb" HTTP fetcher, which uses index files to discover which pack files contain interesting objects.

**RFC**: ([HTML](http://jonas.nitro.dk/gittorrent/rfc.html)) ([XML](http://jonas.nitro.dk/gittorrent/rfc.xml))

## Serialized Protocol Draft ##

TODO: Write about the [extended RFC](http://gittorrent.utsl.gen.nz/rfc.html) by Sam Vilain, which introduces the concept of the _commit reel_.

**RFC**:
([HTML](http://gittorrent.utsl.gen.nz/rfc.html)) ([xml](http://utsl.gen.nz/gitweb/?p=gittorrent;a=blob_plain;f=rfc.xml;hb=HEAD))
([gitweb](http://utsl.gen.nz/gitweb/?p=gittorrent))