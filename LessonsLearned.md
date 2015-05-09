The work on this protocol is very much a learning experience, so it seems natural to carefully look at what lessons can be learned from related protocols.

## Lessons Learned From BitTorrent ##

BitTorrent has quite a long track record by now with many implementations. Since its initial release many extensions have been developed, some of them to fit a very specific use case. Given this rich history of a popular protocol, many lessons can be learned.

  * **Reuse existing protocols and other technology when possible.** Examples are the use of HTTP/CGI for the tracker protocol. This kind of reuse, means that deployment ends up being much easier helping adoption. Security-wise it also makes a lot of sense to extend on already tested implementations.

  * The .torrent files **provide a simple mean of initial resource discovery**. With their distinctive extension and associated media type, they **allow meta information to be distributed by a variety of protocol indifferent methods**, while still mapping to a specific "service" and serving as a sort of finger print for the end result. Compared to other similar P2P systems, this separation of resource discovery (searching for torrents) and resource sharing allows the peer-wire protocol to be simpler and more specialized and reduces communication requirements (e.g. by eliminating the need for flooding).

  * **Keep the core central parts (i.e. the tracker) as simple and stupid as possible.** Central in its worst incarnation means _point of failure_, _management requirements_, _resource intensive_, and therefore have a lot to gain from stripping down and removing complexity. At the same time, any tracker is free to apply more advanced peer matching strategies etc.

  * **Simple peer and piece selection algorithms are good enough.** They reduce peer state and computations and make peers easier to implement, while encouraging fairness. [[1](http://code.google.com/p/gittorrent/wiki/References#Rarest_First_and_Choke_Algorithms_Are_Enough)] Furthermore, the rarest-first policy leads to good replication. [[2](http://code.google.com/p/gittorrent/wiki/References#A_Measurement_Study_of_Piece_Population_in_BitTorrent)]

  * TODO: Security related lessons? Accomplishing data integrity.

## Lessons Learned From DebTorrent ##

The work on defining the DebTorrent protocol provides valuable lessons for which new requirements appear when torrents no longer are static.

  * **Incremental data adds considerable complexity.** It is hard to extend on BitTorrent and maintain its simplicity. The main cause is the additional meta information for defining how generational data is to be handled. This extra dimension of complexity may lead to peers having different views of the data being downloaded.

  * Although data may be replicated several places in the system, **it is necessary to limit the size of the torrent and the amount of meta information** in order to keep the swarm scalable and healthy.

  * TODO: Go through the pros and cons from the DebTorrent wiki page.
  * TODO: maybe by contacting one of the contributors by mail.

## Lessons Learned From First Draft ##

Finally, some lessons from previous work on GitTorrent. Some of this is related to the work done by Sam.

  * **Dependency on git packs as a format for object database serialization is unviable.** Because repositories may contain different kinds of data with highly different compression properties, the heuristics for packing a repository can be tweaked via options. Pack files are thus not stable across different machines.

  * TODO: Authentication of incremental updates.
  * TODO: Mapping an append-only data model to a stream.

## Lessons Learned From Git ##

Since the protocol is targeted from git and the distributed nature of git has posed several interesting challenges and solution, it is relevant to look at what lessons can be learned from a distributed version control system.

  * A guideline often reiterated by Linus is that **good basic data models are more important than anything else.** It is often easier to understand data models than any code that sits on top of it.

  * TODO: Trust model (tags and social network).

## Lessons Learned From Open Source ##

The open nature of the protocol makes it interesting mainly for use in open source development. It may be relevant to come up with some guidelines for how open source operates in terms of distribution and collaboration.

  * The _release early, release often_ mantra encourages an **evolutionary or incremental design, instead of intelligent design**. The lesson is to focus on solving a well-defined, smaller, and often more clearly understood part of the problem. For each iteration the understanding of the system grows and will over time lead to a better design.

  * Of all of the above, perhaps the most important thing to keep in mind is that **it is hard to imagine and foresee how technology is going to be used**. This is true for an "open protocol" like BitTorrent, and more generally for development done in the open. It may even be argued that this is one of the reasons for the success of open source. The lesson is to _design for the future_.