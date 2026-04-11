# My Favorite Algorithms

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

Mike Music: Sepultura - *Roots (1996)*

I learned about Sepultura’s 40th anniversary farewell tour from a podcast called *That’s Not Metal*. I think most people regard Chaos A.D. to be their best album, but i prefer *Roots*. Kinda discovered this some years after it came out, mostly because of the Puerto Rican band Puya.

Erik Music: Kurt Vile and Courtney Barnett - *Lotta Sea Lice* from 2017

One of my favorite parts of programming is implementing (or, occasionally, inventing) algorithms to do various things. It’s pure computer science, usually not a lot of yak shaving or testability issues. This episode is about algorithms that we particularly like, because they are surprising, exemplary, or important.

First though, we should mention that there’s not really a formal definition of what an algorithm really is. Informally, an algorithm is usually described as a series of instructions to be followed to produce a given output for a specified input, eg, the shortest path in a graph, the solution to a system of equations, or a version of a list in a sorted order. However, we also hear more complex processes described as “an algorithm”, like the system that chooses what you see on social media or that decides whether you get a loan for a house. The standard definition also leaves out some subtleties of parallel or distributed algorithms.

[What Is an Algorithm?](https://cacm.acm.org/opinion/what-is-an-algorithm/)

I learned from Phillip Wadler that the term is named for the mathematician [Al-Khwarizmi](https://en.wikipedia.org/wiki/Al-Khwarizmi)

Incontrovertibly, algorithms constitute one of the central subjects of study in computer science. Should we not have by now a clear understanding of what an algorithm is? It is worth pointing out that mathematicians formalized the notion of proof only in the 20th century, even though they have been using proofs at that point for about 2,500 years. Analogously, the fact that we have an intuitive notion of what an algorithm is does not mean that we have a formal notion. For example, when should we say that two programs describe the same algorithm? This requires a rigorous definition!

Things We’re Pretty Sure Are Algorithms

- Mergesort
- Dijkstra’s shortest path

Maybe Algorithms

- PageRank
- Back Propagation

Are These Algorithms?

- Recommender systems
- Decision support tools

## Erik’s Faves

### Radix tree / compact prefix tree / compressed trie / Patricia Trie

- Comments: I always liked heaps and trees. I had this project about 10 years ago where I wanted to learn about different trees. I called it 52-trees. I’ve forgotten most of them unfortunately!
- My favorites were the binary heap (I know, pretty basic, right?) because I just found it so intuitive and easy to remember but also tries, which always made a bunch of sense to me.
- There’s a particular kind of trie called Patricia Trie or a radix tree, but it also has a bunch of other names.
- [Wikipedia](https://en.wikipedia.org/wiki/Radix_tree)
- The Paper (published 1968) is called “PATRICIA --Practical Algorithm To Retrieve Information Coded in Alphanumeric” (it’s an acronym!): <https://dl.acm.org/doi/pdf/10.1145/321479.321481>
- Description:
  + Split input strings along something like a *shared axis* (not official terminology)*.*
  + Example: “testing, treats, trips, teeth, teething”
  + Operations: insert, delete, search
  + Inserts end up with a node that has the prefix made by traversing the tree and any remaining characters as a suffix. Only split a node if there’s multiple children. Any only child is merged with its parent.
  + Search: char by char comparison and “A match is found if we arrive at a leaf node and have used up exactly input-string.length elements”
- Used a ton in web frameworkers: fastest go router, fastest nodejs router, router for axum framework in rust and others use radix trees.
- Radix trees are used for IP Address lookups too
- I wrote the [*tokamak* python http router](https://pypi.org/project/tokamak/) using radix trees when I was surprised to learn that Starlette was doing a linear pass over a list of routes. I didn’t have the will to try to get it *into* Starlette, though, so they’re still doing that (which means FastAPI is still doing it too).
- “Radix trees also share the disadvantages of tries, however: as they can only be applied to strings of elements or elements with an efficiently reversible mapping to strings, they lack the full generality of balanced search trees, which apply to any data type with a total ordering. A reversible mapping to strings can be used to produce the required total ordering for balanced search trees, but not the other way around. This can also be problematic if a data type only provides a comparison operation, but not a (de)serialization operation.”

### HyperLogLog

- First learned about it in Mike’s presentation for Papers We Love, and while I could not sit down today and code it out, it still blows my mind to think about.
- Problem: Count the number of unique elements in a massive set (too big for memory, too big for disk, maybe an infinite stream).
- There is an analogy here to flipping a coin. The observation is that patterns in the result of coin flips can *point to* how long someone has been flipping the coin for. If you flip it 5 times, the probability of seeing a run of heads or tails is 1 / 2^5. Going in the other direction, if you’ve *observed* a run of 5 heads, then that means there have probably been at least 2^5 coin flips. The idea introduced by Morris in 1977 was called *probabilistic counting*: he needed to count something large but he had only 256 bits to do it in!
- You can do the same thing with picking numbers in binary out of a hat. The paper says first that you can hash incoming data to 32-bit binary numbers with uniform probability. A uniform distribution means: pick a random number and *any* number within 2^32 has roughly equal likelihood of being chosen.
- With these binary strings, you will see certain patterns arise, which the paper calls “bit-pattern observables.” You may observe a run of zeroes at the end of a hashed value, and that’s a bit-pattern observable. The count of this run of zeroes is related to the probability of how many input values you’ve seen, and you can keep the highest count you’ve encountered and say “‘we’ve probably seen X inputs based on this.”
- Note: taking a hash means we do not double count items. (This is called a multiset: you may see the same item multiple times but you only count unique items once.)
- This all is based on randomness and probabilities. But here’s where you’ll probably say “a numbering system where you can only count in powers of two sounds like it has limited value to me.” We don’t want to be limited to 2^n values, which wouldn’t be very accurate, so let’s try to make it more accurate with partitioning and math. Instead of just doing this once, we’ll split our stream into buckets, like partitions, and then we’ll calculate something for each bucket’s leftmost 1-bit value (0001 -> 4). We’ll do this:
  1. Take the hash into a binary value
  2. Look at the beginning of the hash, maybe first four bits, and use that to assign it to one of a handful of buckets
  3. We’ll look at the remaining bits and count a run of zeroes.
  4. We’ll compare this run to the max of the bucket and keep the max between new value going into the bucket and whatever the bucket had for max previously. This represents the *add* operation.
  5. Finally, when we want to count we’ll take the *harmonic mean* across *all* buckets. (They tested different means and found that the harmonic mean reduces the effect of large outliers!). This represents the *count* operation. See page 4.
  6. It’s a probabilistic data structure with three operations: *add*, *count*, *merge*
- “As a consequence, using m = 2048 [buckets], hashing on 32 bits, and short bytes of 5 bit length each: cardinalities till values over N = 10^9 can be estimated with a typical accuracy of 2% using 1.5kB (kilobyte) of storage.”
- The name comes from the fact that the buckets (paper calls them registers) are loglogN + O(1) bits
  From the Paper: <https://algo.inria.fr/flajolet/Publications/FlFuGaMe07.pdf>

### Raft Leader Election

- Simple, codable
- Makes sense
- The Paper: <https://raft.github.io/raft.pdf>
- Description of the algorithm:
  + Election Time-out parameter
  + Wait a random number between (timeout/2 - timeout), e.g. 150ms-300ms.
  + Increment epoch (Raft calls this “term”) (there’s viewstamped replication a hat tip to Liskov here!)
  + Send out RPC asking for votes (vote for self)
  + Respond to any request for votes by immediately voting yes (unless already voted for someone else…)

Leaders send periodic heartbeats (AppendEntries RPCs that carry no log entries) to all followers in order to maintain their authority. If a follower receives no communication over a period of time called the election timeout, then it assumes there is no viable leader and begins an election to choose a new leader.

To begin an election, a follower increments its current term and transitions to candidate state. It then votes for itself and issues RequestVote RPCs in parallel to each of the other servers in the cluster. A candidate continues in this state until one of three things happens: (a) it wins the election, (b) another server establishes itself as leader, or (c) a period of time goes by with no winner. These outcomes are discussed separately in the paragraphs below.

A candidate wins an election if it receives votes from a majority of the servers in the full cluster for the same term. Each server will vote for at most one candidate in a given term, on a first-come-first-served basis

- Pathological situations:
  + Lots of elections leading to…
  + Long duration without a working leader
  + Mention FLP? (“a deterministic algorithm cannot achieve consensus in a fully asynchronous message-passing distributed system if at least one process may crash” and with the caveat that there’s no upper bound on how long it may take for a single process to reply.) It’s not guaranteed! But there is a high probability that Raft will work due to randomness and retries: randomness is one way to get around FLP.

## Mike’s Faves

### Fast Fourier Transform (FFT)

Invented by John Tukey (and Cooley), who thought it was too insignificant to publish.

Classic divide-and-conquer approach.

Maybe the most widely used and significant algorithm?

- The Fourier Transform transforms a function from time domain to frequency domain.
- The *discrete* Fourier transform (DFT) works on a function defined at discrete time points
- The naive way to do DFT is O(n^2) where n is the number of points in the series
- The FFT does this in O(n\*logn)
- Technically, only works where n is a power of 2, but fairly easy to get around that

### Metropolis-Hastings Algorithm

Fundamental to Monte Carlo methods, especially as used in scientific applications.

There aren’t a lot of *steps* to Metropolis MC. The key insight is that you can draw a number from a distribution that’s *proportional* to the distribution you’re trying to recreate. That eliminates the need to calculate complicated integrals. For example, you can get samples that reflect the Boltzmann distribution by just computing the numerator of the Boltzmann distribution [exp(-Ei/kT)]. So you:

- Find a new configuration in phase space
- Calculate its energy
- Calculate the Boltzmann numerator to see how probable that state is.
- Compare the ratio of that probability vs the probability of the current state to a uniform random number on [0,1]
- Accept the new configuration if the ratio exceeds the random number

### Kadane’s Algorithm

I mostly like this one because it’s so surprising. An O(n) algorithm for finding a subsequence in an integer array with the largest sum (maximum subarray). Discussed in Programming Pearls. Jay Kadane is alleged to have devised this algorithm in less than a minute.

This is an example of dynamic programming. Kadane’s insight was that

## Algos Pt 2:

Erik Music: Wilco, *Hot Sun Cool Shroud*, it’s pretty good. “Ice Cream” is good. “Annihilation” is great. It sounds just like a song from Wilco.

Mike Music: Father John Misty, *Mahashmashana*

I liked the Yacht Rock documentary on HBO/MAX. FJM seems kind of like the spiritual descendant of Steely Dan (jazz vibes, slick production, cryptic lyrics). A lot of people really dislike him, which is also true of Steely Dan. Anyway, i dig this album, it seems fairly sincere by his standards, and the song-writing is great.

Erik Intro: Last week we talked about our favorite algorithms, including Fast Fourier Transform and Hyperloglog. Today we have a few more algorithms but these are shorter so we can hit more of them for you guys.

Mike does DH:

### Diffie-Hellman Key Exchange

[New Directions in Cryptography](https://ee.stanford.edu/~hellman/publications/24.pdf)

Any encryption system needs a key that is shared by both the encrypter and the decrypter. A key can come in various forms, but for the sake of simplicity let’s say it’s a number or a string of digits.

If you want to send encrypted data to a known trusted party there are safe-ish ways to share a key (eg, a secure communication channel, a courier with a metal briefcase chained to her wrist, etc). However if you want to send encrypted data to any number of parties, most of whom you don’t know, you need a protocol that lets you easily exchange a key without revealing knowledge about the keys you share with other parties. In other words, if you give your secret key to Alice, you don’t want to rely on her not sharing it with Bob.

These problems are what the Diffie-Hellman key exchange algorithm is designed to fix. The algorithm/protocol was invented by Whitfield Diffie and Martin Hellman and published in “New Directions in Cryptography” in 1976. Diffie and Hellman won the 2015 Turing prize. Pictures of Diffie and Hellman from the 70s look like they’re about to lay down a hit folk-rock tune.

This gets a little mathy, so bear with me for a while. First, the algorithm uses what’s called a finite field, basically a finite set on which multiplication, addition, subtraction and division (excluding division by zero) are defined in a particular way. In this case it’s a set of integers. Since it’s a finite set it has a size, usually designated by q[a finite field is also called a Galois field, so in the DH paper it’s called GF(q)]. You can probably see that the operations have to be defined modulo q, so for example if qwere 7, then 3 x 4 is really (3 x 4) mod 7, or 5. In practice qis going to be a much larger number (256 to 2048 bits) and for reason I won’t dwell on, qis a prime number.

In the 1976 paper, D + H start with the premise that every user who wants to exchange encrypted messages chooses a random number X, uniformly from the range [1, q-1] (ie, a random element of the field). The number X must be kept secret, but the algorithm then requires generating the number

Y = ⍺X mod q *[In words, Y is a number ⍺ raised to the Xth power modulus q]*

Where ⍺is what’s called a *fixed primitive element* of the field [Does this need explanation?]. The number Y can be shared with the communicating party. The trick of DH is that Y is relatively easy to calculate, but obtaining X from Y would require solving the *discrete logarithm problem*, or

X = log⍺ Y mod q *[X is the base ⍺ logarithm of Y mod q]*

which is very compute intensive [how hard is this? Might be subexponential algorithms now. Also Shor’s algorithm if you have a quantum computer]. Suppose now that there are users iand jwho want to exchange messages. Each will have their own secret key, Xi and Xj respectively. The shared key value for these two user in the DH algorithm is given by

Kij = ⍺XiXj mod q *[Kij is ⍺ to the power Xi time Xj mod q]*

Of course, as stated above, the Xi and Xj values are secret and are not available to the other user. The DH algorithm requires that each user be able to obtain the Y value for the other user, so user i can get Yj and user j can get Yi. Once each user has the other user’s Y value, they can calculate the key value. For example, user i would calculate it from

Kij = YjXi mod q = ⍺XjXi mod q

Note that anyone intercepting this shared key would also have to solve the discrete log problem to recover Yi or Yj

The DH algorithm is another case that stretches the definition of algorithm. First, it requires cooperation between two parties, and some (unspecified) way to exchange the public Y values. Second, there are really two phases, time-wise: the generation of the X and Y values, and then the key exchange/calculation phase. Third, the crucial aspect of the algorithm is based on a value that can *not* be calculated in polynomial time.

Diffie-Hellman is still an option in the TLS protocol, but it’s what’s called Diffie-Hellman *Ephemeral*, which means that the secret keys are generated and exchanged at the time that the TLS handshake is happening (so there is no permanent secret key, like with an RSA private key). The benefit of a shared key is that TLS can do symmetric encryption.

## The Theory of Algorithms

From “[What Is an Algorithm](https://cacm.acm.org/opinion/what-is-an-algorithm/)” in ACM:

Gurevich argued that every algorithm can be defined in terms of an abstract state machine. Intuitively, an abstract state machine is a machine operating on states, which are arbitrary data structures. The key requirement is that one step of the machine can cause only a bounded local change on the state. This requirement corresponds both to the one-cell-at-a-time operations of a Turing machine and the bounded-width operations of von Neumann machines.

Moschovakis, in contrast, argued that an algorithm is defined in terms of a recursor, which is a recursive description built on top of arbitrary operations taken as primitives. For example, the factorial function can be described recursively, using multiplication as a primitive algorithmic operation, while Euclid’s greatest-common divisor algorithm can be described recursively, using the remainder function as a primitive operation.

[On founding the theory of algorithms](https://www.math.ucla.edu/~ynm/papers/foundalg.pdf)

He gives an example of mergesort with a proof. And then completely dismantles the proof. He writes:

Before going on to learn that most of the preceding section was really meaningless

gibberish, the conscientious reader should re-read it and make sure that, in fact,

it makes perfect sense—except, perhaps, for the last paragraph which turned the

computerese up a bit.

I have called algorithms these purposeful interpretations of equations (1.2)

and (1.1), but computation procedures or effective, deterministic instructions

could do as well (for now)—all these words are used in computer science literature, more-or-less interchangeably.

2.3. Implementations. The second paragraph of Section 1 starts with

the comment that [among sorting algorithms]

. . . the mergesort is (perhaps) simplest to define and analyze, if not

the easiest to implement,

and the last paragraph 1.6 elaborates on the issue. Lots of new words and claims

are thrown around in 1.6: It is asserted that “the mergesort is a recursive algorithm” which can be “expressed in Pascal or Lisp”; that “it is not a simple

matter to implement recursion [in an assembly language]”; that “the implementation of the mergesort requires a lot of space”, etc. The innocent reader should

take it on faith that all of this makes perfect, common sense to an experienced

programmer, and also that very little of it has ever been defined properly. Now

“not the easiest” and “a lot of space” will never be made precise, to be sure, but

this kind of talk suggests that programmers understand and (generally) affirm

the following:

(1) A given algorithm can be expressed (programmed, implemented) in different programming languages, and so (in particular), an algorithm has many

implementations.

(2) Implementations have important properties, e.g., the time and space needed

for their execution.

[What Is an Algorithm? - Yuri Gurevich](https://web.eecs.umich.edu/~gurevich/Opera/209.pdf)

![](data:image/png;base64...)

![](data:image/png;base64...)

Analyzing the recursors of Moschovakis, he writes that these don’t handle everything, for example distributed algorithms.