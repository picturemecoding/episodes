# Shoulders of Giants: Jim Gray

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

## Music

Erik: Joshua Redman *Round Again* (with Brad Mehldau, Christian McBlade, Brian Blade) from 2020. There’s a Joshua Redman record coming out later this month and I’m really looking forward to it, and I had missed this one from 2020. Joshua Redman is a jazz saxophonist and I really like his work. He plays saxophone in this breathy and surprising way, a lot of pathos in his playing. I’m surprised by how it affects me. Looking forward to the new one coming out.

Mike: Alan Sparhawk - “With Trampled With Turtles”

## Intro

We want to do a series of episodes on the topic of databases and we decided to start with a “Standing on the Shoulders of Giants” episode on Jim Gray, since he was pivotal in so many things that we now think of as fundamental in database technology.

Jim Gray won the Turing Prize for his work in databases in 1998. As part of the System R group at IBM in the 1970s he contributed to many foundational ideas in database systems including the concept of transactions and two-phase locking, and he contributed the A,C, and D to ACID. He also contributed to multi-granularity locking. We already mentioned him in our Byzantine Fault Tolerance episode, as the person who named the “Two Generals Paradox”.

Gray had an interesting life with a tragic end, so we’ve organized this episode around the timeline of his life rather than just his papers. Jim Gray was an avid sailor and he mysteriously disappeared on a sailing trip in 2007. He was declared dead *in absentia* in 2012.

I saw him give a talk at UCSD, probably in the 90s, on (i think) his “cyber bricks” idea.

## Timeline

### The Berkeley Years (1961 - 1969)

He got three degrees at Cal

- B.S Engineering - 1966
- MS
- Ph.D, Programming Languages - 1969

Apparently also did work in this time at General Dynamics and Bell Labs

Gray enrolled at UC Berkeley in 1961, grew his hair long, and rented a basement room with a dirt floor for $5 a month. He thought about majoring in philosophy but lost interest when he discovered computers. He signed up for graduate classes without taking the prerequisites, and in 1969, he earned the university’s first PhD in computer science.

From 2005 Microsoft interview called [Behind the Code with Jim Gray](https://www.microsoft.com/en-us/research/video/behind-the-code-with-jim-gray/), he said that philosophers:

Interested in epistemology and “how we understand things.” Problem is, they were using modus ponens and predicate logic as their approach to logic. It was clear it wasn’t going to scale. It was clear to them and to me that it wasn't going to work. Principia Mathematica was recast by Russell and Whitehead into Mathematics. Newell and Simon came along and proved most of the theorems using a computer. They had managed to represent that information in a computer and manipulate information in a way that was very very promising, so I caught the bug and thought “this looks like the way to represent knowledge.”

### IBM/System R (1974-1980)

He went to IBM. The limits to growth model was a big deal in the 70s, and that’s the first thing he worked on. He started trying to test the model and realized: “this model is bogus.” Doing a correct model is extremely difficult.

You sent me a paper called “Eight Transaction Papers by Jim Gray”. I loved this paper because how often do we get an incredible, readable survey of important work by a researcher?

Relational database work came later:

“It was highly controversial within IBM.” “In the beginning there was COBOL. EDP. They had a database task group.” They competed with IBM and IMS (hierarchical) . Network (EDPG) vs hierarchical. It was highly procedural. Off in left-field, were these relational people who said, “both of you groups of people are wrong. It’s crazy to have such a procedural way of poking around through data. You don’t get much data independence, it’s hard to write programs. You should be programming in set theory!” (everyone laughs). It is much much simpler to express the problems you are trying to solve in set theory than in DL1 or DBTG. Both to express and manipulate information. Other people said, “that may be true, but it’s too inefficient.” These problems are huge: 1000s or 10s of thousands of records! Whole disks were 10 megabytes! Turns out Codd was right.

People were building databases and they worked and had concurrency. But people in academia working on concurrency were mostly interested in improving throughput by doing things in parallel. They wanted to do matrix multiply in parallel to speed it up. There is a single right answer.

So we come along and people are doing requests in the DB, and there’s no single right answer. So how do you express that? We started thinking in terms of invariants. So we tried to come u[p with a theory, a set of rules that described the kind of concurrency that would be allowed. It wasn’t straightforward at the time.

If you did the following things, then it’s as though you ran one transaction at a time. Running one a time has no concurrency anomalies. If you run things in parallel and you get an answer that’s the same as one serial execution then you have no concurrency anomalies. So we implemented these locking schemes. And other people implemented them and we learned a lot from each other.

From the *Wired* piece “[Inside the High Tech Hunt for a Missing Silicon Valley Legend](https://www.wired.com/2007/07/ff-jimgray-2/)”:

After a stint at Bell Labs, the scruffy young coder took a job with IBM Research in San Jose. He gamely plunged into the nuts and bolts of data management, considered a dreary backwater even by avid readers of the *Systems Journal*. Back then, inputting a request to a data bank (the word *database* hadn’t caught on yet) required a thicket of query syntax and a team of programmers. But in 1969, an IBM mathematician named Ted Codd had an epiphany. He sketched out a model for a so-called relational data bank that would make it easier to sift relevant needles from haystacks of information.

Gray joined an R&D team that was attempting to turn these theories into functioning software. His work focused on transactions: When should changes be firmly committed to the database? What if one of a pair of transactions fails halfway through? How can two (or 2,000) users access the same data simultaneously without corrupting it? In his research papers, he compared transactions to a marriage contract: What was the right moment to say "I do"?

—

System R also inspired a seminal open source project at UC Berkeley, a relational database called Ingres, built by a crew led by Mike Stonebraker, now a professor at MIT. Though the teams at IBM and Berkeley were officially competing for the same small market, Stonebraker recalls, "Jim was unbelievably smart and very willing to help without getting credit. Most people considered business data-processing to be beneath them. Who would help Bank of America cash checks? No one foresaw how essential databases would become."

No one but an aspiring entrepreneur named Larry Ellison, then a young programmer working at Ampex on a data-storage scheme for the CIA. The agency’s codename for the project: Oracle.

In 1977, Ellison launched a startup called Software Development Laboratories, using System R’s papers as the blueprint for his own software. Then he repositioned his database for the emerging minicomputer market, betting that Big Blue would drag its heels. Ellison bet right, and the rest is Silicon Valley history.

"There was no love lost between Jim Gray and Larry Ellison. They hated each other," one of Gray’s Microsoft colleagues says. But Gray was gracious in public, and he mellowed with the years. He told an interviewer in 2005 that his life had been a "researcher’s dream — you have a lot of fun, you do something innovative, and then people make billions of dollars off of it."

#### From *The Notions of Consistency and Predicate Locks*…

*for the purposes of this discussion we presume that a set of assertions, hereafter calledconsistency constraints, is explicitly defined and we say that the state is consistent if the contents of the entities of the state satisfy all the consistency constraints*

*One may need to temporarily violate the consistency of the system state while modifying it. For example, in moving money from one bank account to another there will be an instant during which one account has been debited and the other not yet credited. This violates a constraint that the number of dollars in the system is constant. For this reason, the actions of a process are grouped into sequences calledtransactionswhich are units of consistency. In general, consistency assertions cannot be enforced before the end of a transaction. In this paper it is assumed that each transaction, when executed alone, transforms a consistent state into a new consistent state*

*we are interested in the problem of running transactions with maximal concurrency by Interleaving actions from several transactions while continuing to give each transaction a consistent view of the system state. In such an environment, each transaction must employ a locking protocol to insure that it and others do not access data which is temporarily inconsistent.*

I’m slightly embarrassed to admit that I didn’t really know about predicate locks. At first I thought maybe that this as an idea that didn’t catch on, but it’s a thing in Postgres: <https://www.postgresql.org/docs/current/transaction-iso.html>

From the summary paper:

- Acquire locks on resources and release the when done
- Acquire locks on all resources you need access to.
- “Consistent concurrent execution of transactions, as if they happened in serial” (serializability)
- The Phantom problem(!): a lock on a query before a record is inserted makes the lock-holder have an inaccurate picture afterwards. Predicate locks is the solution. The whole the index to solve or the whole table. There’s a “hidden step” in there where the select is looking at an index or table-end mark.

#### Granularity of Locks and Degrees of Consistency in a Shared Data Base

Hence a new access mode, intention mode (I), is introduced. Intention mode is used to "tag" (lock) all ancestors of a node to be locked in share or exclusive mode. These tags signal the fact that locking is being done at a "finer" level and thereby prevents implicit or explicit exclusive or share locks on the ancestors.

- A solution for the idea that predicate locks are inefficient (potentially NP hard)

From the summary:

- 0-3 degree transactions: 3rd degree fulfills 2PC and is serializable
- 0: release lock before commit
- Hold short duration read lock
- Hold long duration read lock

### Tandem (1980-1990)

I have some recollection of Tandem. They were a big deal for a while because they made computers that were fault tolerant and so they sold them to banks and other places that needed strong guarantees of reliability. The design (and name of the company) came from the fact that they literally had redundant hardware. Sold to HP. I think this fault tolerant idea influenced what Gray was thinking about at the time.

*1980. Worked at Tandem from IBM (distributed system, fault-tolerant OS). When I left IBM it was a third of a million people. At IBM his first line of code shipped after he left the company, 12 years after he joined! 3 months after I was at Tandem I shipped some code. First 2-3 years was part of his “statute of limitations” for working on dB stuff and not violating intellectual property ownership. It encourages diversity. You have to take 2-3 years off to work on something else.*

#### A Transaction Model

Seems like primarily a formalization of the transaction idea:

Transactions are the mechanism which query and transform the database state. A program P is a static description of a transaction. The consistency constraint of the database is the minimal precondition and invariant of the program. The program may have a desired effect which is expressed as an additional postcondition c'. Using Hoare's notation: c P) C & c'. The execution of such a program on a database state is called transaction on the state. The exact execution sequence of a program is a function of the database state but we model a transaction as a fixed sequence of actions: T = <li=l, ... n> where t is the transaction name, Ai are operations and Xi are entity names.

Not sure if this is the first use of this term:

The first rule (called the *write ahead log* protocol) allows any uncommitted action on stable storage to be undone by applying the undo actions in the UNDO log.

#### The Transaction Model: Virtues and Limitations

The transaction concept emerges with the following properties:

Consistency: the transaction must obey legal protocols.

Atomicity: it either happens or it does not; either all are bound by the contract or none are.

Durability: once a transaction is committed, it cannot be abrogated.

- One of the more interesting bits here to me is that he talks about very long transactions times, like months. Things like contract negotiations or insurance policies.

#### The 5 Minute Rule

The 5-minute rule and the 1-minute rule?

[The 5 minute rule for trading memory for disc accesses and the 10 byte rule for trading memory for CPU time | Proceedings of the 1987 ACM SIGMOD international conference on Management of data](https://dl.acm.org/doi/10.1145/38713.38755)

### Digital (1990-1995)

After a decade at Tandem, Gray moved to Digital Equipment Corporation in 1990, where he started a small laboratory in San Francisco. Over the next four years he consulted with product groups for the Rdb relational database management system and the ACMS transaction-processing monitor. In addition, he and his co-author Andreas Reuter completed the book *Transaction Processing: Concepts and Techniques*. They had begun the work in 1986 as preparation for a one-week seminar; it evolved into a 1000-page book published in 1992.

DEC had a dual-ladder: tech and management tracks. They respected tech. Let the techies influence and even steer the ship. “Unfortunately, the techies drove the company off a cliff.” Started running benchmarks, sorting, transaction processing. They called these “stunts”. He describes the “guru gap,” because gurus can tweak knobs and levers to get great performance, but then they’d go to the product guys and say, “we should do these things by default so you don’t have to be a guru to get great performance.”

### Berkeley Fellowship (1994/1995)

In 1994 Gray resigned from Digital and accepted a Mackay Fellowship at the University of California, Berkeley. He participated in the Sequoia 2000 project, which was designing a Geographic Information System to support global change research.

I worked on Sequoia 2000 when i was at SDSC. That’s when i got into an email argument with Michael Stonebraker

### Microsoft (1995-2007)

[Data Cube: A Relational Aggregation Operator Generalizing Group-By, Cross-Tab, and Sub-Totals](https://arxiv.org/pdf/cs/0701155)

(second most referenced paper)

[The Microsoft TerraServer - Microsoft Research](https://www.microsoft.com/en-us/research/publication/the-microsoft-terraserver/)

This is when he got involved in astronomy also, and eventually build “SkyServer”

1998 - [Jim Gray - A.M. Turing Award Laureate](https://amturing.acm.org/award_winners/gray_3649936.cfm)

From the Wired piece:

When Gray joined Microsoft in 1995, he convinced the software behemoth to launch a research center in San Francisco so that he and his wife wouldn’t have to move to Redmond. "If Jim had wanted a lab in Monte Carlo, we would have built a lab in Monte Carlo," says Microsoft Research chief Rick Rashid. In 1998, Gray’s peers gave him the highest honor in computer science, the A.M. Turing Award.

### Disappearance (2007)

On Sunday, January 28, 2007, Microsoft researcher Jim Gray woke up on his boat, a red 40-foot fiberglass cruiser called Tenacious. The water in Gashouse Cove, a cozy marina in San Francisco Bay, was nearly flat. The 63-year-old programmer phoned his wife, Donna Carnes, who was on an annual vacation with friends in Wisconsin. He said he was heading out to the Farallon Islands, a wildlife refuge 27 miles offshore, to scatter the ashes of his mother, Ann, who died in October.

…

Then Gray and his boat vanished. The Coast Guard received no Mayday call, and Gray’s EPIRB — an emergency radio beacon designed to broadcast a homing signal if it sinks — stayed silent. No sailors in the area reported seeing the boat adrift, and not a single life vest, flashlight, or scrap of debris belonging to Tenacious washed up on local beaches.

From the 2007 Wired Piece after he disappeared in January of year:

“[Inside the High Tech Hunt for a Missing Silicon Valley Legend](https://www.wired.com/2007/07/ff-jimgray-2/)”

Three days after Gray set sail, on Wednesday, January 31, David Swatland [deputy commander of the nearest Coast Guard sector on the West Coast] held a press conference on Yerba Buena Island. "We’ve searched 40,000 square miles of ocean," he said. "We cannot search indefinitely. It is always a hard decision when to break it off."

They announced they would officially call off the search at 1 am Thursday.

From the Wired piece:

Though he felt more at home in industry, Gray would accept the occasional academic chair or fellowship. This gave him the chance to nourish the careers of promising upstarts like Amazon’s Werner Vogels, who asked the programmer to sit on his doctoral committee at the Free University of Amsterdam. Long before Google appeared on Redmond’s radar, Gray was advising Sergey Brin in Mountain View. Asked which of his achievements made him most proud, Gray once said, "The people that I’ve mentored."

His friends in industry organized tech projects to search for him. Werner Vogels had Mechanical Turk search huge volumes of satellite data. Microsoft put up money and Bill Gates let them use his private jet.

### 2012 - Declared Missing But Presumed Dead

[Closure in Disappearance of Computer Scientist - The New York Times](https://archive.nytimes.com/bits.blogs.nytimes.com/2012/05/18/closure-in-disappearance-of-computer-scientist-jim-gray/)

The mystery is interesting on its own because in the most technologically active place in the world a sailor vanished. The story talks about how when a ship goes down, it normally leaves behind oil and floating boat pieces or supplies. Maybe it was hit by a container ship or hit or a whale and sank quickly.

([https://12ft.io/https://www.wired.com/2007/07/ff-jimgray-2/](https://12ft.io/https%3A//www.wired.com/2007/07/ff-jimgray-2/))

Quotes about him:

"Jim’s work inspired us and many other computer scientists to seek out and tackle very ambitious projects," says Google cofounder Sergey Brin. "He never shied away from problems involving large-scale data and computation."

When conference hosts tried to introduce him as a database guru or god, he would say gently, "I’m just a programmer."

Later work at Microsoft:

“Data mining.” How are we going process lots of data from different sources: answer he believes is dataflow processing. Pipeline parallelism. With a kid’s construction set, he builds a DAG and says “you can build elaborate dataflows and get natural parallelism.” Pipeline parallelism and another kind is partition parallelism. He talks about SQL Server IS and BigTable as dataflow programming models. Parallelism where the programs we write are sequential.

40 minutes. Asked about microsoft: it’s a desktop company. David trying to convert them to be all about servers. He asked someone at Netware how they controlled all of the filesystem market when MS controlled the interface. Answer he got was “they don’t get servers.” Instructions on the server are precious. Speed is precious. Simplicity is key to these goals on the server.

“My goal was to do scale-out [horizontal]. For one reason or another, until very recently, we’ve been doing scale-up” [vertical]. You can go a lot further in scale-out than scale-up.

42 minutes. Speed of light is finite. A foot is a nanosecond. That’s in a vacuum. Processor is running at 3Ghz. You don’t have a foot, you’ve got four inches. This is the event horizon. Something happens on one side of this thing, the clock is going to tick before you get to the other side. Processors of the future are golfball size, smoking, you put a lot of power into it and heat dissipation is a big problem. “Smoking hairy golfball problem.” (Hairy: you have to get signals in and out of it). We haven’t gone 3d with our processor architectures. We could actually make 3d processors if we could deal with the heat problem. Predicting that processors will be golfball size “in the next decade” and cooling is going to be a big problem. (this prediction was off…)

TerraServer: research project trying to beg for funds from product people. For eight years we went around with a tin cup begging for support.

48 minutes. Terra server impact (a guy named Tom Barclay). They kept using their project for different microsoft scale demos.

## Links/Timeline

| Paper | Year |
| --- | --- |
| [The Notions of Consistency and Predicate Locks in a Database System](https://jimgray.azurewebsites.net/papers/on%20the%20notions%20of%20consistency%20and%20predicate%20locks%20in%20a%20database%20system%20cacm.pdf) | 1976 |
| [Granularity of Locks and Degrees of Consistency in a Shared Data Base](https://www.cs.cmu.edu/~15721-f24/papers/Granularities_of_Locking.pdf) | 1976 |
| [A Transaction Model](https://infolab.usc.edu/csci599/Fall2008/papers/b-1.pdf) | 1980 |
| [Jim Gray - The Transaction Concept: Virtues and Limitations](https://jimgray.azurewebsites.net/papers/theTransactionConcept.pdf?from=https://research.microsoft.com/~gray/papers/theTransactionConcept.pdf&type=path) | 1981 |
| [A History and Evaluation of System R](https://people.eecs.berkeley.edu/~brewer/cs262/SystemR.pdf) | 1981 |
| [Principles of Transaction-Oriented Database Recovery](http://daslab.seas.harvard.edu/reading-group/papers/recovery.pdf) (the ACID paper) | 1983 |
| [Behind the Code with Jim Gray](https://www.microsoft.com/en-us/research/video/behind-the-code-with-jim-gray/) (Talk at Microsoft) | 2005 |
| [Personal website](https://jimgray.azurewebsites.net/JimGrayHomePageIndex.htm) |  |
| “[Inside the High Tech Hunt for a Missing Silicon Valley Legend](https://www.wired.com/2007/07/ff-jimgray-2/)” (Wired) | 2007 |
| [Jim Gray, Astronomer](https://cacm.acm.org/research/jim-gray-astronomer/) | 2008 |
| [Eight Transaction Papers by Jim Gray](https://arxiv.org/pdf/2310.04601) | 2023 |