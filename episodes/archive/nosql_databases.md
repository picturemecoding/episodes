# NoSQL Databases

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

Erik Music: Turnstile *Never Enough*. Hardcore band from Baltimore, MD. Melodic. Hardcore-inspired. I really liked Glow On and this one feels like a straight extension from that in a good way. They have these arty music videos; there’s like a full-album art video for this record?

Mike Music: There’s been kind of a renaissance in “alt country” lately (i read an article that called it “Y’allternative”. Some of this is maybe inspired by Waxahatchee and MJ Lenderman/Wednesday? Some recent faves:

- Ryan Davis and the Roadhouse Band - New Threats from the Soul
- SG Goodman - Planting By the Signs
- Florry -Sounds Like…

## Intro

Relational databases were designed around business needs like maintaining inventories, keeping track of customer transactions, and maintaining account balances. While they could (and do) have very large transaction volumes, the underlying assumptions about latency, data size and transaction volume were based on a model of businesses interacting with human customers and other businesses.

And then a weird thing happened.

The internet kind of broke databases a little. First, the amount of data coming into databases grew by a bunch. Second, some of the assumptions about consistency changed. Third, the *types* of data that people care about started including wacky stuff like images, audio, video, PDFs, spreadsheets.

Examples of these things:

- Dynamo (and derivatives)
- Bigtable
- Cassandra
- MongoDB
- Redis
- Elasticsearch
- InfluxDB

Some things that come up repeatedly

- Key/Value
- Replication
- Consistent hashing/partitioning
- Availability over consistency
- Bloom filters
- Caching
- Gossip protocols
- [Sorted String Tables](https://medium.com/%40vinciabhinav7/cassandra-internals-sstables-the-secret-sauce-that-makes-cassandra-super-fast-3d5badac8eaf)

Why

- Horizontal scaling
- Write availability

## History

[A Brief History of Non-Relational Databases - DATAVERSITY](https://www.dataversity.net/a-brief-history-of-non-relational-databases/)

Over a couple of years in the mid-oughts, the big internet companies all introduced new database systems designed for their unusual workloads. Google announced Bigtable, Amazon announced Dynamo, and then Facebook introduced Cassandra.

These systems had many similarities, but also differences in the data models.

We have three papers today talking about the above. My favorite papers in this genre are those where after reading it I’m left with a vague sense of, “I could potentially build this if I had enough time and I read this paper enough times…” My least favorite papers are those where I don’t feel like I can build the thing after reading it. (Yay to Dyanmo and Big Table paper, boo to the Cassandra paper.

[Bigtable: A Distributed Storage System for Structured Data](https://static.googleusercontent.com/media/research.google.com/en//archive/bigtable-osdi06.pdf)

*A Bigtable is a sparse, distributed, persistent multidimensional sorted map. The map is indexed by a row key, column key, and a timestamp; each value in the map is an uninterpreted array of bytes.*

*Bigtable can be used with MapReduce [12], a framework for running large-scale parallel computations developed at Google. We have written a set of wrappers that allow a Bigtable to be used both as an input source and as an output target for MapReduce jobs.*

*Bigtable uses the distributed Google File System (GFS) [17] to store log and data files*

*The Google SSTable file format is used internally to store Bigtable data*

*Bigtable relies on a highly-available and persistent distributed lock service called Chubby [8]. A Chubby service consists of five active replicas, one of which is elected to be the master and actively serve requests. The service is live when a majority of the replicas are running and can communicate with each other. Chubby uses the Paxos algorithm [9, 23] to keep its replicas consistent in the face of failure.*

*The Bigtable implementation has three major components: a library that is linked into every client, one master server, and many tablet servers.*

*A Bigtable cluster stores a number of tables. Each table consists of a set of tablets, and each tablet contains all data associated with a row range. Initially, each table consists of just one tablet. As a table grows, it is automatically split into multiple tablets, each approximately 100-200 MB in size by default.*

[Dynamo: Amazon’s Highly Available Key-value Store](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)

*In Amazon’s decentralized service oriented infrastructure, SLAs play an important role. For example a page request to one of the e-commerce sites typically requires the rendering engine to construct its response by sending requests to over 150 services.*

*Query Model: simple read and write operations to a data item that is uniquely identified by a key. State is stored as binary objects (i.e., blobs) identified by unique keys. No operations span multiple data items and there is no need for relational schema*

*Experience at Amazon has shown that data stores that provide ACID guarantees tend to have poor availability. This has been widely acknowledged by both the industry and academia [5]. Dynamo targets applications that operate with weaker consistency (the “C” in ACID) if this results in high availability*

*Many traditional data stores execute conflict resolution during writes and keep the read complexity simple [7]. In such systems, writes may be rejected if the data store cannot reach all (or a majority of) the replicas at a given time. On the other hand, Dynamo targets the design space of an “always writeable” data store (i.e., a data store that is highly available for writes).*

*Dynamo is built for an infrastructure within a single administrative domain where all nodes are assumed to be trusted*

*First, Dynamo is targeted mainly at applications that need an “always writeable” data store where no updates are rejected due to failures or concurrent writes. This is a crucial requirement for many Amazon applications. Second, as noted earlier, Dynamo is built for an infrastructure within a single administrative domain where all nodes are assumed to be trusted. Third, applications that use Dynamo do not require support for hierarchical namespaces (a norm in many file systems) or complex relational schema (supported by traditional databases). Fourth, Dynamo is built for latency sensitive applications that require at least 99.9% of read and write operations to be performed within a few hundred milliseconds*

*Dynamo’s partitioning scheme relies onconsistent hashingto distribute the load across multiple storage hosts*

*Dynamo uses vector clocks [12] in order to capture causality between different versions of the same object. A vector clock is effectively a list of (node, counter) pairs. One vector clock is associated with every version of every object.*

*To achieve high availability and durability, Dynamo replicates its data on multiple hosts. Each data item is replicated at N hosts, where N is a parameter configured “per-instance”*

*Dynamo’s local persistence component allows for different storage engines to be plugged in. Engines that are in use are Berkeley Database (BDB) Transactional Data Store2 , BDB Java Edition, MySQL, and an in-memory buffer with persistent backing store. … The majority of Dynamo’s production instances use BDB Transactional Data Store.*

[Cassandra - A Decentralized Structured Storage System](https://www.cs.cornell.edu/projects/ladis2009/papers/lakshman-ladis2009.pdf)

*A table in Cassandra is a distributed multi dimensional map indexed by a key*

*One of the key design features for Cassandra is the ability to scale incrementally. This requires the ability to dynamically partition the data over the set of nodes (i.e., storage hosts) in the cluster. Cassandra partitions data across the cluster using consistent hashing*

## Reasons for their popularity

Why did these tools get so popular?

- Easy to use (schemaless, JSON, change schemas at the drop of a hat)
- Fast
- High write throughput
- Massively scalable due to distributed design (relational databases are harder to scale)
  + Horizontal scaling vs vertical scaling
- Some of these allow for TTLs (Redis, Cosmos), so you can have data float off into the ether after a duration

## Criticisms of these data stores

- The name NoSQL sucks
Schemaless* so you end up building in a lot of application checks that could otherwise be performed in the database.
- Jepsen critiques: their data guarantees were often not upheld in reality.
- People started with them maybe because they’re easy to use and then used them to try to store relationships (arguably better served by *relational* databases) and ended with complex requests that required a lot of queries to satisfy.
- Data consistency (eventual consistency or “DIY consistency”)
- Security
- RDBMs are built on a foundation of decades of research: don’t cursorily dismiss the knowledge and academic foundation. (Trendiness maybe means that people were potentially assuming traditional databases were unnecessary (did anyone actually make this argument?))

## DBs of the Future and Interesting Trends

Check out the recently released [Stack Overflow developer survey for 2025](https://survey.stackoverflow.co/2025/developers/).

There's a popular [Databases section](https://survey.stackoverflow.co/2025/technology#most-popular-technologies-database-prof) which is like the horse-racey “Which one should you pick?” question:

- Elasticsearch
- DynamoDB
- DuckDB
- Influx
- CouchDB (written in erlang?)

![](data:image/png;base64...)

See also [“admired and desired” databases](https://survey.stackoverflow.co/2025/technology#most-popular-technologies-so-tags-prof)