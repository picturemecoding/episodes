# Hashes and Hash Tables

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

Mike Music: American Recordings by Truman Sinclair.

Somewhere between folk and indie, a little bit of MJ Lenderman, a little bit of Neal Young, maybe some Dylan. Like, emo with banjos and harmonicas. Apparently also the lead guitar for a band called Fat, Evil Children.

Erik Music: *Fantastic Damage* El-P 2002? and Funcrusher Plus 1997. I heard about thes when I was living in Chicago, whenever the Cannibal Ox album came out. And I have revisited them. Frenetic. One of the reviews I read called Funcrusher Plus “apocalyptic”. Fantastic Damage

This episode was prompted by a couple of articles that made me realize that I might not understand hash tables, even though I’ve been using them as if I do for the last 40 years. The first of these was a [Quanta magazine article](https://www.quantamagazine.org/scientists-find-optimal-balance-of-data-storage-and-time-20240208/) that’s now a couple of years old. The second was an [article in Communications of the ACM](https://dl.acm.org/doi/10.1145/3772375) that discusses newer results. The latter is particularly interesting because one of the key breakthroughs came from code written by an undergrad. However, both of these articles talked about improvements in time or space efficiency primarily in a theoretical sense, rather than new implementations of standard data structures.

The articles fascinated me but also confused me because some of the terminology and results did not match what I thought I understood about hash tables. For reference, when I think of hash tables, I’m thinking about associative data structures like Python dicts() and hash maps in Java or Rust.

Here are roughly the prior basics I had about these ideas:

- You have some data and you apply some algorithm to it that maps it to a “bucket”.
- The bucket is an index into a table, where you can store a reference to the data
- A given data item will always hash to the same bucket.
- Since there are a finite number of buckets (usually far fewer than the universe of possible data values), some data items will map to the same bucket (ie, it “collides”).
- The benefit is that you can look up items in O(constant) time.

Note also that for the most part, this doc will discuss basic hashing as opposed to the cryptographic sort of hashing. However, we’ll see later that hash functions that are like cryptographic hashes are used in hash table implementations to prevent “hash flooding”/collision attacks. Also, we won’t talk much about distributed systems or “consistent hashing”.

## History

When I read the above-referenced articles though I had the thought “where did these things come from?”. Hashes/hash tables are not necessarily obvious, so I assumed somebody must have been the first to use them.

The first hash/hash table was apparently invented by Hans Peter Luhn at IBM in 1953. This was what is now called chained hashing, meaning that when a collision happens the data item with the hash value is added to a linked list. Knuth claims that it might also have been the first ever use of linked lists!

Chained hashing is simple but has some drawbacks. One is potentially bad worst-case performance, another is that it must always store references to the data value, so that the data is not “localized” and will cause memory cache misses. However, another of my preconceived notions was this is how hashing always works. It’s the only way I’ve ever implemented hashing and when I was mostly deeply entrenched in these data structures (during my Java days) I’m fairly sure this is how they worked. One of the benefits of chained hashing is that it’s *stable*, ie, the data doesn’t move around within the data structure.

In 1956 Arnold Dumey published a paper called Indexing for [*Rapid Random Access Memory Systems*,](https://bitsavers.org/magazines/Computers_And_Automation/195612.pdf) which talks about how to map data values which have a large possible number of values into a smaller set of random access memory locations. This introduced the idea of “linear probing”, meaning looking for the next available open slot by searching linearly through the memory.

In 1957 Wesley Peterson published "Addressing *for Random Access Storage”*, and gave this concept in general (putting an item in an open slot) the name “open addressing”. Supposedly this technique was already in use within IBM on the 701 system.

Open addressing is the type of hash table discussed in the articles mentioned above, and also (to my surprise) what is used in Python (as of 3.6 at least) and Rust. The main idea in open addressing is that when a collision happens, some logic looks for another open slot in the table rather than adding the value to a linked list. This is called “probing”. The first type of probing was “linear probing”, which means that you just look for the next open slot after the slot with the collision. The main problem with linear probing is “clustering”, which means that certain sections of the table get more filled up than others and this leads to more frequent collisions.

Note that the probing process has to be deterministic because the same process that’s used for insertions will be used for retrieval. This also causes some problems with deletions, so most implementations don’t actually delete entries but rather place “tombstones” in those spots to tell the insertion process to skip them. Finally, open addressing usually favors saving actual data values in the table rather than pointers, because it works better with CPU caching.

In 1972 Jeffrey Ullman published a paper that conjectured that the optimal method for probing for an empty slot is “uniform probing”, roughly meaning that you choose a random slot uniformly across all slots and choose the first one that’s open (also, this means that it is a “greedy” algorithm since it uses the first available slot).

In 1985 Andrew Yao proved Ullman’s conjecture for the “amortized” case, and he conjectured that it was also true for worst-case behavior. The idea that uniform probing was optimal was preached as gospel until 2024. The “worst-case” behavior starts to happen when the so-called *load factor* is high, meaning that most of the slots are already occupied.

It should be noted that “uniform probing” is mostly a theoretical idea, since it requires generating a random permutation of the table locations. People sometimes simulate this with double hashing or *tabulation hashing*. Also note that the model of Ullman and Yao does not allow for elements to be reordered after insertion.

In 2004 Rasmus Pagh introduced [Cuckoo Hashing](https://www.cs.princeton.edu/courses/archive/fall09/cos521/Handouts/universalclasses.pdf). This uses two tables (and two hash functions) and in the case of a collision it can evict an item from its spot and move it to the other table.

A series of papers came out in the early 2020s, the main authors included Michael Bender, William Kuszmaul, and Martin Farach-Colton. These attempted to establish optimal space and time bounds for hash tables. One of the papers they published was called “Tiny Pointers”, which just as it sounds like was about a data structure that implements space efficient pointers (in 64-bit machines, pointers are now 8 bytes). Among the applications were more space efficient hash tables.

An undergrad at Rutgers named Andre Krapivin read this paper and decided to use some of the ideas to implement a hash table. It turned out that Krapivin’s implementation showed that Yao’s conjecture that uniform probing has the best worst-case behavior is wrong. This video is pretty good [FOCS 2024 3B Optimal Bounds for Open Addressing Without Reordering](https://www.youtube.com/watch?v=ArQNyOU1hyE)

Kuszmaul, Krapivin, and Farach-Colton published a paper that describes [elastic hashing and funnel hashing](https://arxiv.org/pdf/2501.02305). Both of these show better worst-case behavior than uniform probing. The funnel hashing variant is also a greedy algorithm, but elastic hashing is not. Again, note that these techniques do not use reordering of table elements, as in say Cuckoo Hashing or Robin Hood hashing.

## Discussion

I think of hashing as generating a fixed-length key from a variable length input, but that’s just a specific case of a more general idea of mapping items from a set A to a set B, where |A| > |B|. (This notation is from Carter and Wegman’s 1979 paper on Universal Hashing). In particular, a lot of the theoretical work is on hashing functions that map from n bits to m bits, where n >= m.

I think i dig hash tables in part because they are fundamentally probabilistic. The basic chaining implementation is often described in terms of the classic [balls-in-bins probability problem](https://en.wikipedia.org/wiki/Balls_into_bins_problem). Much of the theoretical work has been around finding hash functions that produce random results (again, viz Carter and Wegman’s work on Universal Hashing).

I was surprised to find that both Python and Rust’s standard HashMap implementation use open addressing style hash maps. This [talk by Hettinger](https://www.youtube.com/watch?v=p33CVV29OG8) is already 10 years old, but I was surprised by the Python dictionary implementation. [I really liked “catastrophic linear pile-up” from his talk]

There’s a surprising amount of computer architecture knowledge that goes into creating good hash maps. One of the benefits of the open addressing model is that the data is localized so it often doesn’t require a dereference and the data will more likely be in L1/L2 cache. Also, if you watch Hettinger’s talk about his “CompactDict” he talks about how in some cases the entire dictionary index can be loaded in a single cache line. There are specialized hash table implementations from places like google and meta that try to take advantage of caching and SIMD.

Different hash table implementations trade off various properties: space vs time, memory localization vs stability. For example “cuckoo hashing” where two tables and two hash functions are used to deal with collisions allows for guaranteed O(1) lookups, but has the drawback that data has to be moved around.

I found that both Python dicts() and Rust use hash functions that are almost like cryptographic hashes. Python uses [SipHash](https://peps.python.org/pep-0456/#siphash) (invented by Dan Bernstein) and Rust’s HashMap uses something called foldhash (it used SipHash originally). Neither are cryptographically secure like the SHA algorithms, but are hard to reverse engineer and so prevent DoS attacks that seek to generate inputs that hash to the same value.

Python dicts() use a unique version of probing, sort of pseudo-random. Roughly the next slot is chosen with a linear congruential random number generator. The Rust HashMap uses [hashbrown](https://docs.rs/hashbrown/latest/hashbrown/struct.HashMap.html), which in turn is an implementation of a Google thing called SwissTable. This uses what’s called *quadratic probing*. This is one of the implementations that takes advantage of SIMD because it can check a key against a group of 16 entries in one instruction.

## References

[Scientists Find Optimal Balance of Data Storage and Time | Quanta Magazine](https://www.quantamagazine.org/scientists-find-optimal-balance-of-data-storage-and-time-20240208/)

[[2111.00602] On the Optimal Time/Space Tradeoff for Hash Tables](https://arxiv.org/abs/2111.00602)

[Speeding Up Hash Tables | Communications of the ACM](https://dl.acm.org/doi/10.1145/3772375)

<https://dl.acm.org/doi/epdf/10.1145/1734714.1734729>

[[2109.04548] Iceberg Hashing: Optimizing Many Hash-Table Criteria at Once](https://arxiv.org/abs/2109.04548)

[Modern Dictionaries by Raymond Hettinger](https://www.youtube.com/watch?v=p33CVV29OG8)

[FOCS 2024 3B Optimal Bounds for Open Addressing Without Reordering](https://www.youtube.com/watch?v=ArQNyOU1hyE)

[Optimal Bounds for Open Addressing Without Reordering](https://arxiv.org/pdf/2501.02305)