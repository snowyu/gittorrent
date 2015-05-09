The protocol is designed to be an alternative to traditional file transfer protocols such  as TFTP and FTP. It removes the need for large bandwidth capacity on a server, by forcing clients to upload pieces of the file to each other.

The system consists of three distinct entities: a metainfo file, a tracker and a peer swarm.

A peer is launched by the user agent acting on behalf of a user. The peer must become part of a swarm of peers in order for it to be able to carry out the actual download of the file or files. It can become part of an already existing swarm by contacting a tracker, which in return will send back to the peer a list of IP addresses and port numbers of other peers that are already in the swarm. Once connected to the swarm, a peer will act both as a TCP server, by accepting incoming connections from other peers, and as an initiator of connection, by opening a connection to other peers.

A tracker keeps track of the peers that exist in potentially multiple distinct swarms. It does so by internally keeping a list of peers that are connected to each swarm. A tracker is implemented as a HTTP service that responds to GET requests. Usually, the tracker is implemented using a CGI script in a language such as Python or Perl.

The glue that binds a peer to a tracker is the metainfo file. It contains, among other things, the specific URL of the tracker. A peer must fetch this file using some offline method and decode its contents. The metainfo file is composed, and usually put on a website, by the organisation that wants to publish the actual files for download. Apart from the tracker URL, the metainfo file contains other information necessary for peers to download and verify data from other peers.

For the purpose of sharing, the data is viewed as a single stream of bytes, even though it might consist of multiple files. The metainfo file contains the hierarchy as well as filenames that a peer uses to reassemble the file. The data stream is now partitioned into smaller pieces of equal size, perhaps apart from the last piece. For each of these pieces the metainfo file contains a 20-byte SHA1 hash value. This value is used by each peer to confirm that the data received from a peer is in fact the same data that was intended by the publisher of the metainfo file. A peer usually does not download an entire piece, but
splits it into an implementation defined number of blocks. The peer may download the blocks from multiple peers in the swarm. Once a piece is complete, the peer must check its integrity and discard the piece if it fails the check.

In order for a swarm to initialise correctly there must be an initial peer that can serve all the pieces to other peers. This initial peer is usually set up by the publisher of the metainfo file, and is called the initial seeder. Any other peer that manages to acquire a complete copy of the files to download is also called a seeder.

## References ##

  * [The official protocol specification](http://www.bittorrent.org/protocol.html).
  * [Community maintained protocol specification](http://wiki.theory.org/BitTorrentSpecification).
  * [Paper with RFC-like protocol specification](http://gittorrent.googlecode.com/files/Fjeldsted-Fonseca-Reza-2005.pdf). The above description is taken from this work.
  * [Brian's BitTorrent FAQ and Guide](http://dessent.net/btfaq/).
  * Other RelatedWork.