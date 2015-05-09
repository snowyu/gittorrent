The road map, as presented here, has not been divided into milestones but presents tasks that it might be relevant to investigate further. They should be entered into the issue tracker.

# Preliminary Project Tasks #

## Repository Design ##

Gather information about how a git repository works in terms of file and data structures and semantics.

  * File formats and data structure: Describe the main repository on-disk file formats as well as in-memory data structures with the goal of understanding how host-level distribution is achieved, for example how endianess is handled in lowlevel files and how platform differences are worked around.

  * Update semantics: Map in detail the update semantics of a repository. More specifically, the resulting change of a given action, such as committing and tagging. The goal is to understand how atomicity and the append-only nature are achieved.

  * Revision traversal: Describe how revision listing is specified and done in git and how the revision traversal engine works. If possible, use set-based operations to make it more abstract. The goal is to understand the characteristics of how the object database can be queried and propose a method for how subsets of the database can easily and cheaply be exchanged by peers.

## Repository Statistics ##

Create tools to get information from real-world project repositories. As a basis, [tools](http://gittorrent.googlecode.com/files/git-stat-tools.zip) from a previous GitTorrent project can maybe be used.

  * Object database statistics: Create tools for gathering statistics for the object database of various opensource projects. The goal is to get an idea of how the various object types relate to each other in terms of amounts and delta-size between snapshots. For example how big a percentage do commit objects make up of a repository.

  * Project repository updates: Create tools to monitor opensource projects with respect to how often updates appear in key repositories and how they change over time, for both the content (objects) and references (tags and branches). E.g. are changes different before a release, how often do new tags appear compared to regular branch updates.

  * Hierarchic structure: Create tools for investigating how different repositories relate to each other, e.g. how Linus' tree relates to the tree of Jeff Garzik. Git's distributed model encourages forking resulting in many repositories sharing most of the data. Each repository can be seen as a set with objects overlapping between different repository sets. The goal is to get a sense of familiarity and ancestry between a neighborhood of repositories.

If there is time, some further statistics on the performance of the current git protocols, to motivate a new protocol:

  * How well are HTTP repositories packed? Will users end up downloading a lot of extra data because these repositories are rarely or too often packed incrementally?

  * How much is the "meta information" overhead of the "git protocol handshake" where the client and server exchanges information about which commits each one has?

## Lessons Learned ##

This task tries to summon up the work done in the different preliminary stages and should hopefully end up with some useful guidelines for the following design. Some of the git related work on statistics could be presented to the git community for discussion.

  * Write about LessonsLearned by taking a look at proposed ProtocolSpecifications, related protocols and other RelatedWork.

  * Look at statistics and capture some of it in heuristics.

  * Go through the list of OpenQuestions.