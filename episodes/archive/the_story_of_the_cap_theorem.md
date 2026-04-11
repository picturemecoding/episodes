# The Story of the CAP Theorem

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

### Intro

Welcome to Picture Me Coding

Mention: email -> podcast@picturemecoding.com

Mention: bluesky, website,

Mention: started a new Pateron ([patreon.com/PictureMeCoding](http://patreon.com/PictureMeCoding)) for us to support some of our expenses associated with running the show. Eventual goal is to have a professional edit the show, but we need your support to do that! For $4/mo, you can sponsor our show and we will read your name aloud on the show to thank you for your support.

### Music

Erik music: Billy Woods, Kenny Segal *Hiding Places*. I heard about this guy from our friend Randy and I listened to a handful of records and this weird record is the one that really stuck with me. They’re all weird, to be fair, but musically none of them grabbed me the way this one did. The first song on the record is called “Spongebob”. You read the lyrics for this song and it is *going places*. It’s a weird song. It’s a weird album, but I get it stuck in my head. In “Spongebob” he references these war-torn places, but I think the song may be about survival in war. There’s this odd stream of consciousness that I don’t really know how to parse but it feels dense and interconnected. Here is an example from “Spongebob”:

Get fished out the hole like Saddam / Tough guys won't go alive, get found unarmed / An object in motion stays in motion, I wait 'til the sea calm / Slaves like hammer, Braun, ships was yea-long / It's too late for qualms with the hammer in the palm

Mike music: Jeff Parker, ETA IVtet, *The Way Out Of Easy*

Came out in mid-December. Parker is a guitarist who collaborates with all sorts of musicians, though i suppose he’d be mostly classified with jazz. This album is 4 long tracks, all instrumental. I really love this album. It’s jazz i think, but slightly different in style and composition than traditional jazz. The tracks are diverse too. The opener, called Funkadelick, lives up to it’s name, but the second track (Late Autumn) sounds almost like Phillip Glass.

## The Story Part 1

“I want to tell you a story Mike…”

The story begins in the summer of 2000 at a conference called the ACMSymposium on Principles of Distributed Computing. That year the conference was held in Portland, Oregon, from July 16-July 19th. They actually refer to this conference by the acronym PODC: Principles of Distributed Computing, and they’ve been running it in the summers since 1982. This year it’ll be held at Huatulco, Mexico: we should go! This conference is where the “Edsger W. Dijkstra Prize in Distributed Computing” comes from: you’ve heard of this before because at the one I want to tell you about in 2000, they gave it to Leslie Lamport for his clocks paper. It’s a big conference.

So imagine! It’s the year 2000 in Portland Oregon and all of these distributed computing researchers have gathered to talk about their field. They still have [a 2000 PODC website up](https://www.podc.org/podc2000/home.html) from the organizing of the conference which describes how you can get to venue at the Portland Marriott City Center from the airport. I enjoyed looking at this website because it feels like traveling back in time: the text of it is very present tense for the year 2000: all-white background with black text on every page. No pictures. For example, they have a page talking about Portland itself and [the page starts with](https://www.podc.org/podc2000/pdx.html) the header “BLUEBERRIES!!!” in all capital letters and three exclamation marks. It was a gentler, friendlier time. It feels like a small community.

On the first day of the conference at 5pm Leslie Lamport gave a tutorial on writing TLA+. I looked up the weather that day and it was 90 degrees outside, so people were coming in to this workshop all sweaty from having picked blueberries and they’re ready to learn for the first time how to do TLA+!

But then, on the morning of July 19th (a Wednesday) a guy named Eric Brewer from UC Berkeley gave an “invited talk” called “Towards Robust Distributed Systems.” It was only about 60 degrees outside when he gave this talk, by the way. You can still see the synopsis on their [website](https://www.podc.org/podc2000/brewer.html). The synopsis reads:

Current distributed systems, even the ones that work, tend to be very fragile: they are hard to keep up, hard to manage, hard to grow, hard to evolve, and hard to program. In this talk, I look at several issues in an attempt to clean up the way we think about these systems. These issues include the fault model, high availability, graceful degradation, data consistency, evolution, composition, and autonomy.

These are not (yet) provable principles, but merely ways to think about the issues that simplify design in practice. They draw on experience at Berkeley and with giant-scale systems built at Inktomi, including the system that handles 50% of all web searches.

This became known as the CAP Conjecture (or even Brewer’s Conjecture) and very soon afterward, this idea became pretty well-known. You’ve probably heard of it as the *CAP Theorem*, which is the next part of our story we’ll get to. For now, we’re just calling it the CAP Conjecture. The reason it’s a conjecture here is in this sentence: “these are not yet provable principles.”

[Digression on conjecture vs theorem: question for Mike]

Background on Brewer: he had joined Cal pretty young, he was 26. He said in later interviews that he had grad students older than him. He had joined Cal faculty right at the rise of the web and he was interested in clustering of machines as the way to solve these massive internet-scale problems that he thought were right around the corner. He also guessed search engines were the first problem to really need large scale, so he wanted to do research to contribute to this moment in time for technology. His company Intoki was working on what we would now call a CDN: content distribution network, and they were building these massive databases to serve traffic. This was pre-cloud and even mostly pre-VM.

[Mike] Maybe before we proceed we can define what we mean by a *distributed system*? Once upon a time programs ran on a single computer. There were no networks and when you wanted to work on a larger problem, you just got a bigger computer. Then some wiseguy got the idea that you could achieve certain goals by building systems that ran on multiple computers connected by a network, and that ruined everything. Imagine for example that you’ve got a database management system. If you get too many requests to either write to or read from that database, then a single machine will eventually slow down. It might crash or get unplugged and now your system doesn’t work at all. But if you replicate the data on multiple machines, if you distribute the data, you can scale better and have better guarantees that stuff still works if one machine explodes. In exchange, you get a nest full of hornets. Mind hornets.

[yeah, you get mind hornets, but you also get to go rub elbows and pick blueberries with Leslie Lamport in Portland]

Here’s a basic rundown of CAP Conjecture:

- It’s an acronym and each letter stands for some property we’d like our system to have
- C: “Consistency”
- A: “Availability”
- P: “Partition Tolerance”

Originally (and even today) it’s contrasted with the acronym ACID, which are guarantees we typically seek from relational databases. “Atomicity, Consistency, Isolation, and Durability”

It was also contrasted with BASE which is an acronym for “Basically Available”, “Soft state”, and “Eventually consistent”, an acronym I don’t like as much because there’s no clean correspondence of a single letter to a concept (BA -> …).

- Consistency means all nodes agree
- Availability means they respond all the time (but they can actually respond very slowly)
- Partition tolerance means there’s some kind of network failure which may prevent the nodes from talking to each other. Short version: this means you have multiple nodes.

Anyway, CAP Conjecture says, you can only have 2 out of 3 of these properties. If I run multiple nodes I may have “partition tolerance” but then I can’t have *both* availability and consistency. It’s surprising and a little weird, actually, that you can’t have both? I think programmers don’t like being told they can’t have all the things.

Brewer says at the time he was making the argument that “you have to make this choice.” He said people would always assume they could have everything they wanted, often using snapshots as an example. “Snapshots by definition are not consistent.”

So, it’s often convenient to be able to look at a system, to *know* you can’t have all three of C, A, and P, and then think about what “kind” of system you have:

- CP
- CA
- AP
- AC

We often play this game where we look at new database systems (or their marketing materials!) and ask which letters of the CAP Theorem they purportedly claim to uphold. Is it CP or AP? (Because a new database these days without partition tolerance may be surprising).

You can actually look at the Powerpoint slidedeck for the keynote that Brewer gave at the 2004 PODC; it’s available here: <https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf>

![](data:image/png;base64...)

![](data:image/png;base64...)

In an interview a couple of years ago, Brewer mentioned that he first stated the CAP Conjecture while teaching a course at UC Berkeley in 1998. He had been working on CDNs for Inktomi and the database they built was optimized for availability over consistency. He talked about this database at a big DB conference and he said people were dismissive because he wasn't prioritizing consistency.

Fun facts about Eric Brewer: eventually he leaves Cal faculty and joins Google about 10 years after this PODC conference. While at Google he gets involved in borg and eventually works on developing and arguing for open-sourcing kubernetes! He said in an interview that the Google deployment platform “App Engine was too abstract. VMs were too specific.” He called Kubernetes the “goldilocks sweet spot” because it can run anything. It abstracts the machine, system dependencies and allows app devs to shove their stuff into a place. He said, “For kubernetes I felt it was too big a change in industry, not to get other players to participate.”

I want to peek behind the curtain a bit with my story. Here’s my thesis for you Mike: the CAP Theorem is the most widely known distributed systems result in our industry. If you’re a working programmer and you’ve heard of a single distributed systems topic, I’m betting it’s CAP Theorem. What do you think about my thesis?

Yeah, so the question is: why? Why *this* idea? So I want to answer this question below.

## The Story Part 2

Okay, now, we’ve briefly discussed the CAP theorem. Let me take you back now a couple decades earlier to 1985.

This was the year that saw publication of an extremely important paper in our field, a foundational and ground-shaking idea that inspired *loads of research in distributed systems theory*. The paper is called “Impossibility of Distributed Consensus with One Faulty Process”.

The consensus problem involves an asynchronous system of processes, some of which may be unreliable. The problem is for the reliable processes to agree on a binary value. In this paper, it is shown that every protocol for this problem has the possibility of nontermination, even with only one faulty process. By way of contrast, solutions are known for the synchronous case, the “Byzantine Generals” problem.

This paper is very well-known and it’s often informally called “FLP” after the names of its authors Fischer, Lynch, and Patterson. It’s kind of crazy to think that in a specific model of a distributed system, if you have oneprocess crash, then it’s *mathematically impossible to achieve consensus.* That’s incredible. Remember PODC, the conference in Portland, the Djikstra prize? The very next year, 2001, they won the Djikstra prize for this 1985 paper.

Here’s a chunk from the introduction of this paper:

In this paper, we show the surprising result that no completely asynchronous consensus protocol can tolerate even a single unannounced process death. We do not consider Byzantine failures, and we assume that the message system is reliable – it delivers all messages correctly and exactly once. Nevertheless, even with these assumptions, the stopping of a single process at an inopportune time can cause any distributed commit protocol to fail to reach agreement. Thus, this important problem has no robust solution without further assumptions about the computing environment or still greater restrictions on the kind of failures to be tolerated!

[Moreover] Our impossibility result applies to even a very weak form of the consensus problem… Our system model is rather strong so as to make our impossibility proof as widely applicable as possible…

Q: What is an “async system of processes”?

A: No answers are sent immediately. It’s like I receive a message and don’t respond when I receive it. It just goes into a queue. Later on, I may send out my own response. Communication links themselves are assumed to be reliable. The key feature is processors may take *any amount of time* to receive, process and respond to an incoming message.

Q: “What is consensus?”
A: It involves three things:

- Termination: they must all complete
- Agreement: every correct process has the same value
- Validity: the value must have been one proposed by one or allcorrect processes (weaker consensus: value proposed by one of the processes)

And here’s the surprising conclusion: there is no deterministic solution for achieving consensus if *one* process fails! As they put it in the paper, they’re showing “the surprising result that no completely asynchronous consensus protocol can tolerate even a single unannounced process death.” *It is surprising*!

I was watching a talk from a few years ago by Nancy Lynch where she was talking about her work and when she talked about this paper, she said, “what if you just wait for the stopped process to wake back up and then tell it at that time about the decided value? It doesn’t work. There’s no deterministic solution.”

Still surprising, right?!

The key thing to imagine here is that there’s a kind of stripped-down *formal model* of a set of processes communicating over reliable channels. This isn’t meant to be like an analogy for the internet. It’s more a model the authors can use to demonstrate various properties.

And, actually, I get pretty lost understanding the proof in the paper, but people in the field say it’s a creative, novel proof, with a technique that has been used elsewhere. I have here [an informal description (from the authors)](https://www.podc.org/influential/2001-influential-paper/) of what they’re doing in this paper:

The intuition behind the impossibility proof is pretty simple: Initially, either decision, 0 or 1, is possible. Assume a correct algorithm. At some point in time the system as a whole must commit to one value or the other. That commitment must result from some action of a single process. Suppose that process fails. Then there is no way for the other processes to know the commitment value; hence, they will sometimes make the wrong decision. Contradiction!

This simple argument turned out to be surprisingly difficult to make precise. The subtlety of the argument became apparent to us when we tried to explain the result to others: many people expressed skepticism and disbelief. It took a lot of time, and careful polishing of the proof on our part, before the correctness of the result became generally accepted.

Weirdly, solutions exist if you use randomization (which is where things like Raft come in!).

Anyway, Paxos, Raft, and various other results in distributed systems come out of the FLP result. It’s a hugely important part of the work in this field.

The citation when they won the Djikstra award for this paper says the following

This result has had a monumental impact in distributed computing, both theory and practice. Systems designers were motivated to clarify their claims concerning under what circumstances the systems work.

On the theory side, people have attempted to get around the impossibility result by changing the system assumptions or the problem statement

…

This result has been extremely influential both because of its implications and because of the proof technique it pioneered. Its importance stems from the fact that consensus lies at the heart of many practical problems, including atomic commit, leader election, atomic broadcast, and the maintenance of consistent replicated data. The unsolvability result has motivated researchers to explore models that retain as many of the asynchronous model’s attractive features as possible, while making consensus (and related practical problems) solvable. These explorations include randomization, partially synchronous models, and unreliable failure detectors.

Sidebar here, FLP is an *impossibility result*: it proves that some particular set of properties is *impossible* to achieve *under specific conditions*. Using those conditions, we can show how building a system with a particular set of goals may be actually impossible in practice.

It’s like if people weren’t aware of what was actually, practically impossible, they could still have been trying to build systems that promise these impossible things. We can also use this to understand when outlandish claims about some system have been made. If someone promises a distributed database that sounds like it can do impossible things, we should pay particular attention to those claims!

But, I want to tell you about one of those authors of FLP Nancy Lynch. She’s the “L” in FLP!

Born in 1948, she’s been a professor at MIT since 1982 and she’s won a huge number of awards. She actually wrote the standard textbook on distributed systems called *Distributed Algorithms*.

Here’s a couple of lines from her 1989 paper “[A Hundred Impossibility Proofs for Distributed Computing](https://groups.csail.mit.edu/tds/papers/Lynch/podc89.pdf)”:

What good are impossibility results, anyway? They don’t seem very useful at first, since they don’t allow computers to do anything they couldn’t previously. Most obviously, impossibility results tell you when you should stop trying to devise or improve an algorithm. This information can be useful both for theoretical research and for systems development work.

So I think this is really fascinating and part of the appeal of this work. What good is it for us programmers to be told, “what you are trying to do in this specific instance is *impossible*.” Further, what does it mean to be told, “it’s mathematically impossible.” It’s not just someone’s opinion about how it’s very difficult or will take a long time. These arguments are about like a ceiling, a limit to information in our universe.

## The Story Part 3

### Intro

Welcome to Picture Me Coding

Mention: email -> podcast@picturemecoding.com

Mention: bluesky, website,

Mention: started a new Patreon ([patreon.com/PictureMeCoding](http://patreon.com/PictureMeCoding)) for us to support some of our expenses associated with running the show. Eventual goal is to have a professional edit the show, but we need your support to do that! For $4/mo, you can sponsor our show and we will read your name aloud on the show to thank you for your support.

### Music

Erik: Ben Wendel The Seasons… Saxophone whale sounds

Mike: Doechii - *Alligator Bites Never Heal*

### Part 3: Lynch Proves CAP Theorem

So now back to the CAP Theorem. We have FLP in 1985, we have the CAP Conjecture presented at PODC in 2000. “These are not (yet) provable principles, but merely ways to think about the issues that simplify design in practice.”

Now, Nancy Lynch sees Brewer’s conjecture and realizes at some point, “Aha, this may be a special application of our result from 1985.” So in 2002 she and Gilbert *prove* the conjecture and it *becomes* the CAP Theorem (graduating from a “conjecture”). The paper with this proof is called “[Brewer's conjecture and the feasibility of consistent, available, partition-tolerant web services](https://users.ece.cmu.edu/~adrian/731-sp04/readings/GL-cap.pdf)”. The paper isn’t very long and we encourage everyone to try to read it.

The model:

Consistent: “Atomic or linearizable consistency is the condition expected by most web services today. … there must exist a total order on all operations such that each operation looks like it completed in a single instant.” [It’s easiest for users to understand because it looks like all events happen on a single node.]

Available: all requests receive a response (but we haven’t said how long they can take…)

PT: Messages can be lost. Network partition is like all messages are lost from the partitioned node. Thus: consistency “even though some messages may not get delivered.” OR availability (respond to all requests) “even though some messages may get lost.”

The mathematical impossibility is this: the proof by contradiction is very similar to FLP: it’s *possible* to construct a sequence of events which show that our guarantee of all properties is violated. This is especially easy with *any* message loss. Example: two nodes can’t talk to each other (partitioned), one receives a read, the other a write. If they can’t talk to each other, the node handling the read is responding without awareness of the write. Trivially, then, they can’t promise to be consistent.

I would argue that the proof here is a bit different than FLP. It’s still a proof by contradiction, and an impossibility result, but unlike FLP it posits a setup that’s fairly plausible (whereas the sequence of events in FLP is possible but fairly contrived and unlikely).

What’s most confusing here to me is the “partition tolerance” part. In Theorem 1 they assume a CAP system, then prove that it is not consistent, so what they have seems to be an AP system. But what they mean by partition tolerant seems to be trivial. It is equivalent to two nodes that are available to all clients but not connected to each other. I guess it’s technically true that they are “tolerating” partition since they allow the nodes to be unable to communicate, but would a database vendor claim to be partition tolerant if they just provided a database system where writes only applied to isolated nodes?

Okay, what if you can guarantee that no messages are lost? Well, in an async algorithm we have no way to know if messages have been delivered or if they’re just going really slowly. So this “no messages are lost” model is roughly equivalent to the model where *all* messages are lost. It will take some time for messages to be delivered, so we can wait-and-confirm (for consensus) or just send things off and hope for the best (availability). It’s impossible to have both. In fact, having both would be “an atomic execution”, which can’t really exist. They use the word atomic here to mean something like “instantaneously.” That’s the part that’s easy to see as likely to be impossible. You can also construct Lamport diagrams where reads/writes to each node are delayed. This makes it easy to test out different scenarios and try to work out why it can’t be consistent or if you force consistency, the system can’t be available (because it’s gotta do a bunch of work to *confirm it’s consistent* and that work takes time!).

I said this last week but it’s fascinating to me both with FLP and the CAP Theorem that there’s a “mathematical impossibility.” It’s like some kind of hard limit to information, like the Halting Problem or Gödel’s Incompleteness or like the speed of light in physics: we will never break this particular metaphorical sound barrier!

Side note: there’s a funny comment from the earlier “Hundred proofs” 1989 paper by Lynch: she says these proofs are “easy” to do (compared to other proofs in the field, I guess…)

Impossibility proofs are much easier in our area than in most others. This is because the limitation of local knowledge is the fundamental fact about the setting in which we work, and it is a very powerful limitation.

I think what she’s getting at here is that distributed systems can be constrained and simplified in various ways? You don’t have to worry about infinities or instantaneous global effects. Memories are not infinite, networks are not instant, a change at node A doesn’t immediately affect all other nodes in difficult to determine ways.

She’s like Good Will Hunting over here with the impossibility proofs: “this is easy for me,” she’s saying to us.

It is probably true that most systems developers, even when confronted with the proved impossibility of what they’re trying to do, will still keep trying to do it. This doesn’t necessarily mean that they are obstinate, but rather that they have some flexibility in their goals. E.g., if they can’t accomplish something absolutely, maybe they can settle for a solution that works with “sufficiently high probability”. In such a case, the effect of the impossibility result might be to make a systems developer clarify his/her claims about what the system accomplishes.

Now, we see it mentioned everywhere! After this CAP Theorem paper by Gilbert and Lynch, it’s like there’s suddenly a certain *virality* to this idea. Over the following 20 years, it’s mentioned everywhere. Do you have a new distributed database? You are expectedto talk about how it relates to the CAP Theorem. [Provide examples…]

It’s a hugely successful mathematical theorem.

Interviewing for a job where you will be dealing with distributed systems? Having heard of the CAP theorem is table stakes.

So, if you’re a working software engineer and you’ve heard of a thing from the area of distributed systems, you’ve probably heard of the CAP theorem.

*Mike - Even though i think most journeymen programmers aren’t familiar with CAP, it is interesting how this particular theoretical result has a certain tech-bro cachet that almost nothing else does. The only things i can think of with similar mojo are, maybe, Merkle Trees, P vs NP, or Turing Completeness. Also similar in that most people using these terms probably don’t really understand them. I suspect it’s partly because of its marketing value? I think it’s a little bit like “eventual consistency”. Saying “our database has eventual consistency” sounds better than saying “our database does not provide consistency”. Similarly saying we are “CP” in the context of the CAP theorem is better than saying “we can’t guarantee availability”. I also think maybe it became popular as an interview question because of the “theorem” part. A bit of “math envy” maybe?*

### The Story Part 4

[But the story of the CAP Theorem doesn’t really end there. ]

[erik] Now, I like the CAP theorem because it’s a convenient way to organize information about *what can go wrong*. We know with distributed systems that loads of stuff can go wrong! Tons of things can fail! The CAP Theorem gives us a convenient way to partition our knowledge of the world into some neat categories. Most of the time I only need to think about CP or AP:

Examples of databases that are commonly described as CP or AP:

- Raft, leader-based: CP
- DynamoDB, Cassandra, Riak: AP
- Postgresql: trick question!

Some people have complained about this. Google spanner, for instance, “we’re mostly CAP”. Strictly speaking if you asked if we’re CAP (from their blog post…)

- “The purist answer is “no” because partitions can happen and in fact have happened at Google, and during some partitions, Spanner chooses C and forfeits A. It is technically a CP system… In practice, we find that Spanner does meet this bar [CAP], with more than five 9s of availability.”

Well, Martin Kleppeman has an article from 2015 called “[Please Stop Calling Databases CP OR AP](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)” where he argues that we should stop talking about the CAP Theorem for databases.

Kleppeman is a CS professor. If you haven’t heard of him, he’s the author of *Designing Data-Intensive Applications*, a great book.

Anyway, in this article, Kleppeman says:

If you want to refer to CAP as a theorem (as opposed to a vague hand-wavy concept in your database’s marketing materials), you have to be precise. Mathematics requires precision. The proof only holds if you use the words with the same meaning as they are used in the proof. And the proof uses very particular definitions.

He says first of all that CAP is based on a highly specific model:

The CAP theorem doesn’t just describe any old system, but a very specific model of a system: the CAP system model is a single, read-write register – that’s all. For example, the CAP theorem says nothing about transactions that touch multiple objects: they are simply out of scope of the theorem, unless you can somehow reduce them down to a single register.

For one example, one thing that always sounds weird to me is talking about “partition tolerance.” The distributed systems literature does reference network partitions, but it seems more generic to talk about straight-up failures: if I have a bunch of nodes participating in some distributed algorithm and trying to make progress and one of them fails, is “network partition” the best name for that? I never felt very confident raising this concern myself.

Kleppeman raises it for me, though!

Partition Tolerance (terribly mis-named) basically means that you’re communicating over an asynchronous network that may delay or drop messages. The internet and all our datacenters have this property, so you don’t really have any choice in this matter.

He writes up above this:

The only fault considered by the CAP theorem is a network partition (i.e. nodes remain up, but the network between some of them is not working). That kind of fault absolutely does happen, but it’s not the only kind of thing that can go wrong: nodes can crash or be rebooted, you can run out of disk space, you can hit a bug in the software, etc. In building distributed systems, you need to consider a much wider range of trade-offs, and focussing too much on the CAP theorem leads to ignoring other important issues.

So partition tolerance is badly named and not even a property we can get away from. He goes on to say that the “Consistency” and “Availability” terms are also not very useful for *what we normally want to talk about*:

Consistencyin CAP actually means linearizability, which is a very specific (and very strong) notion of consistency. In particular it has got nothing to do with the C in ACID, even though that C also stands for “consistency”.

Availability in CAP is defined as “every request received by a non-failing [database] node in the system must result in a [non-error] response”. It’s not sufficient for some node to be able to handle the request: any non-failing node needs to be able to handle it. Many so-called “highly available” (i.e. low downtime) systems actually do not meet this definition of availability.

Kleppeman says:

Also, the CAP theorem says nothing about latency, which people tend to care about more than availability. In fact, CAP-available systems are allowed to be arbitrarily slow to respond, and can still be called “available”. Going out on a limb, I’d guess that your users wouldn’t call your system “available” if it takes 2 minutes to load a page.

What’s linearizability? This is a *consistency model* like “serializability” or “read-your-writes”. It’s a set of guarantees or properties we want our system to uphold with respect to consistency in a distributed system. In effect, it’s a way of answering the question “exactly *how* consistent do you want this system to be?” See [the Jepsen models](https://jepsen.io/consistency/models) for more examples.

Anyway, with linearizability, it’s pretty darn strict, but it makes intuitive sense to us as something desirable. It's like if events A and B occur with A first, in a linearizable system B will occur in the state of the world after A has completed. It’s not like a locking transaction in a database but stronger than that. A more common and weaker consistency guarantee would be serializability where you guarantee that concurrent evaluations yield the same result as if they were done in some order. Linearizable is nice, but difficult. The other common consistency is serializability. That’s*the* consistency model that ACID is typically about.

If you think about the typical database things: isolated transactions, locking rows, etc. They’re trying to achieve serializability because it doesn’t matter if row object 11 changes only after row object 8 has finished changing if those rows have nothing to do with each other!

Kleppeman writes:

If you want to provide linearizable semantics (CAP-consistency) in your database, you need to make it appear as though there is only a single copy of the data, even though there may be copies (replicas, caches) of the data in multiple places. This is a fairly expensive guarantee to provide, because it requires a lot of coordination.

Postgresql provides serializability. Oracle provides neither!

The fact that we haven’t been able to classify even one datastore as unambiguously “AP” or “CP” should be telling us something: those are simply not the right labels to describe systems.

Kleppeman references a paper called “[Highly Available Transactions: Virtues and Limitations](https://www.vldb.org/pvldb/vol7/p181-bailis.pdf)” describing transaction guarantees:

Despite its narrow scope, the CAP Theorem is often misconstrued as a broad result regarding the ability to provide ACID database properties with high availability; this misunderstanding has led to substantial confusion regarding replica consistency, transactional isolation, and high availability.

Eventually CAP Brewer says “this lead to the NoSQL movement”; it freed people up to prioritize non-ACID guarantees.

To be fair, Brewer says (in his 10 years paper) people misunderstand the spectrum and tradeoffs part. “The binary part captures people's attention”. “People don't talk about how to recover from partition tolerance”.

## References // Links

- [Wikipedia on the CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem)
- Brewer’s “[Towards Robust Distributed Systems](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf)” (slideshow of the talk!)
- FLP Paper: [Impossibility of Distributed Consensus with One Faulty Process](https://groups.csail.mit.edu/tds/papers/Lynch/jacm85.pdf) (1985)
- Lynch: “[A Hundred Impossibility Proofs for Distributed Computing](https://groups.csail.mit.edu/tds/papers/Lynch/podc89.pdf)” (1989)
- Lynch and Gilbert *prove* CAP Conjecture: “[Brewer's conjecture and the feasibility of consistent, available, partition-tolerant web services](https://users.ece.cmu.edu/~adrian/731-sp04/readings/GL-cap.pdf)” (2002)
- Martin Kleppeman “[Please Stop Calling Databases CP OR AP](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)”
- “[Highly Available Transactions: Virtues and Limitations](https://www.vldb.org/pvldb/vol7/p181-bailis.pdf)”
- Background: “[A Brief tour of FLP Impossibility](https://www.the-paper-trail.org/post/2008-08-13-a-brief-tour-of-flp-impossibility/)”
- Background, Brewer Interview in 2015: <https://medium.com/s-c-a-l-e/google-systems-guru-explains-why-containers-are-the-future-of-computing-87922af2cf95>
- Background, Brewer interview with Software Engineering Daily 2023: <https://softwareengineeringdaily.com/2023/05/12/cap-theorem-23-years-later/>
- [A Theoretical View of Distributed Systems: Nancy Lynch](https://www.youtube.com/watch?v=TAxnJptSWXc) (2021 Talk)

##

##

## Appendix A: What Does FLP Really Say?

The problem that FLP addresses is a version of the consensusproblem, specifically getting a set of processes running on different computers connected by a network to agree on the value of a variable (consensus definition above is “terminination, agreement, validity”). Typically the value will be changed in one process and then messages will be sent to the other computers to inform them of the change. If everything goes well, all the processes will end up with the same value, that is, they will achieve consensus. A correct implementation *must* end at some point with a *correct* value.

Much of distributed systems theory is devoted to the cases where things do *not* go well. Algorithms that try to ensure consensus in distributed systems are designed to be fault tolerant, in expectation of dicey networks, failing servers, and even malicious participants.

In FLP the messages sent to other machines are *asynchronous*, which here means that nobody knows when or in which order they will be delivered. It also means that you can’t rely on timeouts to decide that a process is faulty. The model of FLP also assumes that the messaging system *is* reliable, that messages are delivered and delivered only once, so that a faulty processor is one that simply stops processing the messages. For a system like this *it had beenassumed* that consensus could still be established with enough non-faulty processes.

Instead, the FLP result proves that even if there’s *only one* faulty process, there’s no reliable series of steps that will guarantee consensus. Because FLP shows that no algorithm can guarantee consensus in an asynchronous system with even one faulty process, it is called an *impossibility result*.

FLP says, “you can’t guarantee an outcome in all scenarios: it’s always *possible* to hit a sequence of events which invalidate your attempts at consensus!” In other words, we cannot build an algorithm that is guaranteed to terminate with a correct result. A deterministic solution that would *always work* in the face of failure is what’s demonstrated to be impossible here.

## Appendix B: How Does the Gilbert-Lynch CAP Theorem Paper Prove the Theorem?

I was thinking about it in terms of a Lamport diagram: it’s possible to construct a diagram where messages between participants are delayed to such a degree that the events cannot be called “atomic” (as if they happened in a single instant) or *linearizable*.