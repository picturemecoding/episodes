# (Conflict-Free Replicated Data Types)

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

# (Conflict-Free Replicated Data Types)

CRDTs were first described around 2007-2009 in a series of papers by Marc Shapiro and others. Then in 2011by Shapiro et al. they wrote “[Conflict-free Replicated Data Types](https://pages.lip6.fr/Marc.Shapiro/papers/RR-7687.pdf)” and they also wrote the comprehensive description of them as well as a handful of what have become canonical data structures in a paper out of INRIA that everyone cites called [“A comprehensive study of Convergent and Commutative Replicated Data Types”](https://dsf.berkeley.edu/cs286/papers/crdt-tr2011.pdf). The “comprehensive” paper is long (50) pages, but everything in it appears in discussions of these over the succeeding 15 years up to now.

## Description

Informally: we can define data structures which naturally combine in ways that make them converge in a distributed system. Why would we want to do this?

Here’s a section describing their motivation from the shorter paper’s introduction:

When network delays are large or partitioning is an issue, as in delay-tolerant networks,

disconnected operation, cloud computing, or P2P systems, eventual consistency promises better availability and performance. An update executes at some replica, without synchronisation; later, it is sent to the other replicas. All updates eventually take effect at all replicas, asynchronously and possibly in different orders. Concurrent updates may conflict; conflict arbitration may require a consensus and a roll-back. This weaker consistency is considered acceptable for some classes of applications. However, conflict resolution is hard. The literature offers little guidance on designing a correct optimistic system. Ad-hoc approaches are brittle and error-prone…

They mention here the “concurrency anomalies of the Amazon Shopping Cart” which are described in [the dynamo paper](https://www.amazon.science/publications/dynamo-amazons-highly-available-key-value-store).

We propose a simple, theoretically-sound approach to eventual consistency….

The word “eventually” is a window into where we’re going: we are in the world of “eventual consistency”, *AP systems*, my friends.

Not just “eventual” but “Strongeventual consistency”, which sounds pretty good…

Our system model, Strong Eventual Consistency or SEC, avoids the complexity of conflict resolution and of roll-back. Conflict-freedom ensures safety and liveness despite any number of failures. It leverages simple mathematical properties that ensure absence of conflict, i.e., monotonicity in a semi-lattice and/or commutativity. (p. 1)

At first I thought CRDTs were arbitrary data types, that you could build whatever data structures you want and turn them into CRDTs, but in fact only a handful of actually solid CRDTs have been described. And they have to conform to specified mathematical properties: if you demonstrate they do, then you get strong eventual consistency.

From the later section “Results” (section 3):

A SEC replica is always available for both reads and writes, independently of network conditions. Any communicating subset of replicas of a SEC object eventually converges, even if partitioned from the rest of the network. SEC is weaker than strong consistency but nonetheless provides the well-defined guarantee of strong eventual convergence. SEC provides an extreme form of fault tolerance, as a SEC object tolerates up to *n − 1* simultaneous crashes. Remarkably, SEC does not require to solve consensus. (p. 9, section 3: “Results”)

## Mathematical Underpinnings

Check out [this great article](https://www.cs.utexas.edu/~rossbach/cs380p/papers/Counters.html) for a lot of good background information on the G-counter and PN-Counter and some Lamport diagrams and a lattice that shows them converging:

From the [soundcloud post](https://developers.soundcloud.com/blog/roshi-a-crdt-system-for-timestamped-events):

The tl;dr on CRDTs is that by constraining your operations to only those which are associative, commutative, and idempotent, you sidestep a lot of the complexity in distributed programming. That, in turn, makes it straightforward to guarantee eventual consistency in the face of failure.

CRDTs are based on Algebraic Data Types! But they add some cool algebraic structures and require mathematical properties…

- Join semilattice
- Associative, commutative, idempotent

State-based vs operation-based.

- State-based CRDT (called CvRDT for “Convergent…” in the original paper)
- Operation-based CRDT (called CmRDTfor “Commutative…” in the original paper)

These terms “state-based” and “operation-based” became a lot more popular after the paper came out, so you don’t see the “Convergent” and “Commutative” names so much.

Rules for Convergent CvRDT (state-based) from the paper (p. 14):

- Monotonic semilattice
- Merge forms a LUB

Rules for Commutative CmRDT (op-based) from the paper (p. 14):

- Ops delivered in causal order
- Delivery order exists
- Concurrent updates commute

The term that shows up a lot related to the semilattice in the paper is the *least upper bound* (also called the “join”). In other words, this is the place where the data structure *converges*.

For each state-based example, we have the obligation to prove that its states for a monotonic semilattice and a that *merge* computes a LUB. For each op-based example, we must demonstrate that a delivery-order exists and that concurrent updates commute. (p. 14)

Note: you can emulate one type of object from the other.

State-based mechanisms are simple to reason about, since all the necessary information is captured by the state. They require weak channel assumptions, allowing for unknown numbers of replicas. However, sending state may be inefficient for large objects; this can be tackled by shipping deltas, but this requires mechanisms similar to the op-based approach (p. 11).

On the other hand…

Specifying operation-based objects can be more complex since it requires reasoning about history, but conversely they have greater expressive power. The payload can be simpler since some state is effectively offloaded to the channel. Op-based replication is more demanding of the channel since it requires reliable broadcast, which in general requires tracking group membership. (p. 12)

### Order

The concept of order comes up a lot in distributed systems. In a single threaded system the operations that modify a data structure are naturally sequential, so it’s not a big deal. With concurrent threads you have to control the order of operations to make sure you don’t end up with race conditions. With distributed systems things get really hard because you have multiple processes updating multiple things using messages over potentially flaky networks.

You hear two terms with respect to order:

- Total order: a set of things with some operation that can compare every item in the set, eg, integers and <=; or events and “happens before”
- Partial order: similar to total order except that some elements might not be comparable. For example, in a set of events with operation “happens before”, some events might be concurrent. Or, the set of all applicants to a university (you can rank the physics applicants but you wouldn’t compare a physics applicant to a literature applicant)

Both of these things are *ordered sets*.

In the CRDT papers they talk about *lattices*. Lattices essentially add bounds to ordered sets. A subset of an ordered set can have a *least upper bound* and also a *greatest lower bound*. You can define an operator for two elements of an ordered set that gives the lowest upper bound for two elements in that set, which can in the general case return nothing (ie, no LUB). This operation is usually called *join (v)*. There can also be an operator that gives the greatest lower bound, which is called *meet (^)*.

A *lattice* then is defined as an ordered set where for every element x,y in the set both x v y and x ^ y exist. The specific thing in CRDTs is a *join semilattice*, which means that the first part (x v y) is satisfied but not necessarily the second part.

Part of the reason why these things are nifty in the context of distributed data structures is because the join operation commutes, so X v Y = Y v X, implying that you arrive at the same place regardless of the order in which things happen.

### Examples

- GCounter
- PNCounter
- Glist
- MV-Register (multi-value register)
- GSet
- 2PSet (can add and then remove but never add again: g-set + remove-set, “tombstone set”)
- Orswot (observed-removed set, add has precedence)
- Last-Write-Wins element set
- PN-Set (add increments, remove decrements)
- 2P2P-Graph
- Add-only monotonic DAG
- Add-Remove Partial Order -> WOOT data structure for concurrent text editing
- Map

### Concurrent Text Editing

Peer-to-peer cooperative text editing is a particularly interesting use case of an add-remove order. Users sharing a document repeatedly insert a text element or remove one. Using a CRDT for this ensures that concurrent edits never conflict and converge, even for users who remain disconnected from the network for long periods, as long as they eventually reconnect.

A sequence for text editing is a totally ordered set of elements, each composed of a unique identifier and an atom (e.g. a character, a string, an XML tag, or an embedded graphic), supporting operations to add an element at some position, and to remove an element. (p. 33, shapiro et al)

## Appears In

- Riak: [Riak DT map: a composable, convergent replicated dictionaries](https://dlnext.acm.org/doi/abs/10.1145/2596631.2596633)
- Redis: ​​<https://redis.io/active-active/>
- CosmosDB: “[Last write Wins](https://learn.microsoft.com/en-us/azure/cosmos-db/conflict-resolution-policies)” and patch document (source?)
- Soundcloud: “[Roshi: a CRDT system for timestamped events](https://developers.soundcloud.com/blog/roshi-a-crdt-system-for-timestamped-events)”
- Akka: “[Akka Distributed Data”](https://doc.akka.io/libraries/akka-core/current/typed/distributed-data.html)
- [Fly.io](http://fly.io) recently released this crdt-on-top-of-sqlite gossip service discovery service called corrosion: https://superfly.github.io/corrosion/

## Discussion Question: why so popular?

So what’s the appeal of these data structures? We’re not all doing collaborative editing…

A few reasons in my opinion. One, I believe that CRDTs have a certain attractiveness to people working on distributed systems problems because they seem *easier* than consensus protocols. If you compare with a Paxos or Raft consensus, to correctly implement it you have to code this pretty complex and messy protocol. These are certainly not *intuitive* protocols to build. On the other hand CRDTs suggest that if you just the data structures right, you don’t necessarily need a complicated protocol (op-base causal delivery, though…). Data structures are easier to remember than complicated protocols.

In addition, eventual consistency is pretty good for a lot of apps. We don’t need linearizable consensus and we’re often okay with our data being slightly stale. And Paxos and Raft have complex failure modes: you need 2n+1 nodes in order to tolerate n failures in Paxos. With CRDTs we’re welcoming all comers: send your changes and we’ll take them! (again, qualifications for op-based…)

## References

- 2011 Shapiro et al “[Conflict-free Replicated Data Types](https://pages.lip6.fr/Marc.Shapiro/papers/RR-7687.pdf)”
- 2011 Shapiro et al [“A comprehensive study of Convergent and Commutative Replicated Data Types](https://dsf.berkeley.edu/cs286/papers/crdt-tr2011.pdf)
- UTexas Rossbach [Lecture on Counters](https://www.cs.utexas.edu/~rossbach/cs380p/papers/Counters.html)
- ([Roshi repo](https://github.com/soundcloud/roshi) is pretty interesting.)

Related work: Lattice Agreement

- 2012 “[Generalized Lattice Agreement](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/Generalized20Lattice20Agreement20-20PODC12.pdf)”
- 2020 “[Linearizable State Machine Replication of State-Based CRDTs without Logs](https://arxiv.org/pdf/1905.08733)” (they call this “crdts-paxos”! They separate updates from reads: updates are immediate but reads go through some paxos-consensus rounds until the state has converged.) There are lots of useful references in this paper. “Similar to CRDTs, values proposed in GLA belong to a join semilattice—a partially ordered set that defines a join (least upper bound) for all element pairs. In contrast, for generalized consensus it is not required that such a join always exists. This difference makes generalized lattice agreement an easier problem to solve.”