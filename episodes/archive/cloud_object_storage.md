# Intro

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

Erik Music: Aesop Rock *I Heard It’s a Mess There Too*. I like this whole record a lot actually. He has a song on here called “The Cut” that I really like and another called “Full House Pinball”. That’s the one where he starts talking about heaven and then compares it to sitting on a chair with his cat on his lap working and he says in the chorus, “It’s a mess out here. I heard it’s a mess there too.” He’s rejecting the many possible heavens people imagine for some simpler goals. I like it. Still funny. Still bonkers poetry. I am in awe of the work and relate to the speaker.

Mike: Raphael Feuilletre - Visages baroques. Classical guitar, Back and other baroque composers.

ANNOUNCE: Check out our 2025 PMC playlist on Spotify– we will link in show notes.

# Cloud Object Storage

Amazon’s S3 (Simple Storage Service) is probably the most used cloud service and it seems to be more and more important for key services. Google, Azure, and Oracle (?) have their own versions of object storage. Is it more than just “the infinite FTP server in the sky?”

- How do these things work? Are they just big-ass disk drives?
- How have they changed software?
  + Do they make cloud computing obligatory since they can’t be replicated locally?
  + The rise of disaggregated databases
- Are they too big to fail?

## History and Technical Underpinnings

- S3 launch on March 14th, 2006
- “S3 is an object storage service with an HTTP REST API. There is a frontend fleet with a REST API, a namespace service, a storage fleet that’s full of hard disks, and a fleet that does background operations. In an enterprise context we might call these background tasks “data services,” like replication and tiering. S3 today is composed of hundreds of microservices…” (from here)
- “[2023] S3 holds more than 280 trillion objects and averages over 100 million requests per second.” “It also performs more than 4 billion checksum computations per second.” (link).
- “As of 2025, S3 stores 500 trillion objects, 100s of exabytes of data, serves 200 million requests per second, and peaks at about 1 petabyte per second in bandwidth.” ([wiki](https://en.wikipedia.org/wiki/Amazon_S3))

### But How Does It Work?

The operations S3 allows on objects are simple CRUD operations:

- PUT
- GET
- DELETE

We start with a fleet of API servers: clients send requests to these things. These API servers then propagate these requests to a system that manages writing stuff *onto actual disks*.

Here’s a key point: they use hard disks. Remember those? The biggest ones they can buy (26TB in 2023). WD now sells 36TB HDD. At scale, Hard drives are less than half the cost of solid state. Per TB, HDD costs between $10-$20, and solid state drives (according to [diskdrives.com](http://diskdrives.com), which aggregates drive prices across amazon) are close to $60 per TB! We’re talking *millions of disks* are in use at S3, so the savings are vast…

To understand the technical hurdles added by this cheap HDD storage fleet, we have to consider that a hard disk is a mechanical spinning disks (multiple) with a magnetic read head: the read head has to physically move to the correct disk and then the disk has to spin to the proper location for the data before the head can even start to read it. *All of this movement takes time*.

From a [2023 article on betterstack](https://betterstack.com/community/guides/databases/aws-s3-petabyte/) which describes this in great detail:

- Moving to the correct disk can take up to 25ms
- Spinning the disk can take another 8ms
- Reading the data itself (once all the parts are in place) can take an additional 2ms.

From [an interview with Andy Warfield](https://www.allthingsdistributed.com/2023/07/building-and-operating-a-pretty-big-storage-system.html), VP and distinguished engineer at S3, he says:

“If you are doing random reads and writes to a drive as fast as you possibly can, you can expect about 120 operations per second.”

Andy Warfield gives this incredible 747-flying-extremely-low over blades of grass analogy for a hard drive:

Imagine a hard drive head as a 747 flying over a grassy field at 75 miles per hour. The air gap between the bottom of the plane and the top of the grass is two sheets of paper. Now, if we measure bits on the disk as blades of grass, the track width would be 4.6 blades of grass wide and the bit length would be one blade of grass. As the plane flew over the grass it would count blades of grass and only miss one blade for every 25 thousand times the plane circled the Earth. That’s a bit error rate of 1 in 10^15 requests.

### How To Write

The interesting thing is that they optimize the write path with durability and the expense of finding stuff on their many many hard disks.

They split objects into shards and store those on random servers in their fleet.

The goal with writes is to just write in one giant log, one object after another, because then you can avoid moving the mechanical disk heads around! You want to avoid writing in random locations on the disk, so instead they just write data in one continuous stream.

In addition, with millions of disks to choose from, they have to pick disks that spread out the load (ideally, the least-used disks are first for writes). To pick a low-used disk, they randomize:

S3 uses a simple but remarkably effective randomized algorithm based on a load-balancing principle called "The Power of Two Random Choices."

They randomly pick two servers and then select the least-used of those two. At their scale this ends up distributing load incredibly well (because millions of random selections ends up saturating all possibilities).

### What About Reads

On the read side, reading could be tough if everyone reads from the same disks at the same time, because that involves moving around, so they have to deal with hotspots.From the Andy Warfield interview,. He says:

So, with all this in mind, one of the biggest and most interesting technical scale problems that I’ve encountered is in managing and balancing I/O demand across a really large set of hard drives. In S3, we refer to that problem as heat management.

By heat, I mean the number of requests that hit a given disk at any point in time. If we do a bad job of managing heat, then we end up focusing a disproportionate number of requests on a single drive, and we create hotspots because of the limited I/O that’s available from that single disk. For us, this becomes an optimization challenge of figuring out how we can place data across our disks in a way that minimizes the number of hotspots.

The read load from their customers is bursty, but it aggregates across millions of customers in a pretty uniform pattern.

[This kind of blows my mind]:

That burst of requests can be served by over a million individual disks. That’s not an exaggeration. Today, we have tens of thousands of customers with S3 buckets that are spread across millions of drives. When I first started working on S3, I was really excited (and humbled!) by the systems work to build storage at this scale, but as I really started to understand the system I realized that it was the scale of customers and workloads using the system in aggregate that really allow it to be built differently, and building at this scale means that any one of those individual workloads is able to burst to a level of performance that just wouldn’t be practical to build if they were building without this scale.

…

In storage systems, redundancy schemes are commonly used to protect data from hardware failures, but redundancy also helps manage heat. They spread load out and give you an opportunity to steer request traffic away from hotspots. As an example, consider replication as a simple approach to encoding and protecting data. Replication protects data if disks fail by just having multiple copies on different disks. But it also gives you the freedom to read from any of the disks. When we think about replication from a capacity perspective it’s expensive. However, from an I/O perspective – at least for reading data – replication is very efficient.

We obviously don’t want to pay a replication overhead for all of the data that we store, so in S3 we also make use of erasure coding. For example, we use an algorithm, such as Reed-Solomon, and split our object into a set of k “identity” shards. Then we generate an additional set of m parity shards. As long as k of the (k+m) total shards remain available, we can read the object. This approach lets us reduce capacity overhead while surviving the same number of failures.

### Formal Verification of ShardStore

Going deeper into just one component of this system, the application that keeps track of shards is called ShardStore.

From the paper “[Using Lightweight Formal Methods to Validate a Key-Value Storage Node in Amazon S3](https://assets.amazon.science/77/5e/4a7c238f4ce890efdc325df83263/using-lightweight-formal-methods-to-validate-a-key-value-storage-node-in-amazon-s3-2.pdf)” (2021), they talk about this application being 40k lines of Rust, just one of hundreds of microservices involved in running S3!

The ShardStore key-value store is used by S3 as a storage node. Each storage node stores shards of customer objects, which are replicated across multiple nodes for durability, and so storage nodes need not replicate their stored data internally. ShardStore is API-compatible with our existing storage node software, and so requests can be served by either ShardStore or our existing key-value stores

A production storage system combines several difficult-to-implement complexities [25]: intricate on-disk data structures, concurrent accesses and mutations to them, and the

need to maintain consistency across crashes. The scale of a realistic implementation mirrors this complexity: ShardStore is over 40,000 lines of code and changes frequently.

It’s an LSM Tree (log-structured merge tree), with the actual shards outside of the tree.

The LSM tree maps each shard identifier to a list of (pointers to) chunks, each of which is stored within an extent. Extents are contiguous regions of physical storage on a disk; a typical disk has tens of thousands of extents. ShardStore requires that writes within each extent are sequential, tracked by a write pointer defining the next valid write position, and so data on an extent cannot be immediately overwritten. Each extent has a reset operation to return the write pointer to the beginning of the extent and allow overwrites.

They then describe crash consistency.

ShardStore’s extent *append* operation, which is the only way to write to disk, has the type signature:

fn append(&self, ..., dep: Dependency) -> Dependency

both taking as input and returning a dependency. The contract for the append API is that the append will not be issued to disk until the input dependency has been persisted.

They can make dependency graphs out of these dependencies, passing them around and evaluating inside out.

For formal verification they wrote these “reference models”, which were stripped-down versions of the program logic. They give the example that instead of a full LSM tree, they use a HashMap. This is what I found cool: they could have used another formal verification language, but they write:

However, we found that by writing reference models in the same language as the implementation, we make them easier for engineers to keep up to date. We also

minimize the cognitive burden of learning a new language and mapping concepts between model and implementation.

The goal was to hand these tools off to engineers, not formal verification experts! They then use property-based testing to validate that properties of the modeled object are actually present in the reference model.

## Why Do Companies Use It?

- It’s cheap, compared to, say, EBS
  + Price depends on latency and frequency of access (tiering)
- It’s elastic/unlimited
- Very reliable (11 9s)
- Works well with data retention policies (ie, keeps infrequently referenced data and can automatically expire data)

## How Do Companies Use It?

- Dumb database, shove blobs into it and reference them from other databases
- Static websites/content
- Database backups
- Analytics databases

### Analytics/Data Lakes

S3 and other cloud storage has had a profound effect on database technology, especially for analytics and data warehousing/ETL workloads. They have enabled the “data lake” concept, where you keep data in its original form and use schema-on-read tools like Presto/Athena/Trino.

So-called “diskless” databases…

In conjunction with other technologies like Arrow and Parquet, they’ve enabled the creation of open table formats like Delta Lake and Iceberg, which have transformed the world of analytics and ETL.

## References

- [Exploiting Cloud Object Storage for High-Performance Analytics](https://www.vldb.org/pvldb/vol16/p2769-durner.pdf)
- <https://arxiv.org/abs/2406.00550v1>
- [Using Lightweight Formal Methods to Validate a Key-Value Storage Node in Amazon S3](https://assets.amazon.science/77/5e/4a7c238f4ce890efdc325df83263/using-lightweight-formal-methods-to-validate-a-key-value-storage-node-in-amazon-s3-2.pdf)
- [Building and operating a pretty big storage system called S3 | All Things Distributed](https://www.allthingsdistributed.com/2023/07/building-and-operating-a-pretty-big-storage-system.html)
- <https://betterstack.com/community/guides/databases/aws-s3-petabyte/>
- [AWS re:Invent 2021 - Deep dive on Amazon S3](https://www.youtube.com/watch?v=FJJxcwSfWYg)
- [Reed-Solomon error-correction codes](https://en.wikipedia.org/wiki/Reed%E2%80%93Solomon_error_correction)