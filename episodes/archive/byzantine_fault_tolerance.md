# Byzantine Fault Tolerance

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

Mike Music: Radio DDR -Sharp Pins

Erik Music: Bon Iver, SABLE, fABLE (adult contemporary that radically pushes the genre forward. I don’t just want to listen to yacht rock. I want to listen to revolutionary yacht rock. Mind-melting yacht rock.)

Not long after I started my career in programming, I bought the book *The Puzzling Adventures of Dr Ecco* by Dennis Sasha. The book follows the exploits of Dr. Jacob Ecco, a sort of mathematical Sherlock Holmes, who solves tricky problems for his clients. Most of the puzzles are computer science problems disguised as business or personal challenges. Sasha is a mathematician and computer scientist who teaches at the Courant Institute. He invented Dr. Ecco as an entertaining way to pose these problems in the context of a fictional narrative. It’s a lot of fun, I really enjoyed it. Until Chapter 6.

The first section of Chapter 6 is entitled Knowledge Coordination 1. It poses the following problem:

*There are two allied Generals A and B whose camps are on opposite sides of a ridge. They can communicate with each other only by carrier pigeon. The pigeons sometimes get lost or killed by birds of prey.*

*The generals must decide whether or not to attack the enemy the following morning. Whatever they decide, either they both attack or neither attacks, because an attack by just one would lead to a disaster.*

*Now, suppose General A decides that the moment is propitious so he sends a message via carrier pigeon to General B saying “Attack at dawn”. Without receiving confirmation General A won’t attack so he asks General B to acknowledge the message. General B sends his acknowledgement.*

*Will any sequence of acknowledgements and counter-acknowledgements lead them to attack?*

Up to this point in the book I’d worked through most of the problems. Sometimes I found a wrong or incomplete answer, but usually an hour or so sufficed. I worked on this one for almost two weeks, absolutely convinced that there must be a sequence of messages that would lead to an attack and I was failing to see the trick that would be obvious once revealed.

If you recognize this problem, you already know the punch line: there is no sequence of messages that will guarantee both parties will attack. In fact, this is a well-known computer science problem, sometimes called The Two Generals Problem, that describes a fundamental challenge with distributed systems.

Question: Where does this problem come from? I just know about the Lamport paper. I didn’t think Lamport invented the problem, though?

The first statement of this problem is now thought to have been made in the paper *Some Constraints and Tradeoffs in the Design of Network Communications*. The paper is about inter-process communication mechanisms (IPCM), which we can think of as two processes on separate machines communicating via a network. A section talking about the necessary properties of an IPCM says:

*Status InformationOne of the facilities provided by a well-designed IPCM is to return information to the participants of a transaction as to its outcome. This status return facility is quite burdensome, especially in computer networks, and it was proposed to eliminate it altogether in such situations. Although this would result in considerable simplification, it can be shown (see appendix) that if the system itself did not provide this facility, there is theoretically no protocol that the users themselves may devise to fill this gap and totally eliminate their anxiety*

The referenced Appendix has this statement of the coordination problem, described in terms of communications among gangsters:

*A group of gangsters are about to pull off a big Job. The plan of action is prepared down to the last detail. Some of the men are holed up in a warehouse across town, awaiting precise instructions. It is absolutely essential that the two groups act with complete reliance on each other in executing the plan.*

The switch from gangsters to generals happened several years later in a paper by Jim Gray, a chapter in a conference proceedings called *Notes on Data Base Operating Systems*. As an introduction to the Two-Phase Commit Protocol, Gray describes what he calls “The Generals Paradox”.

*There are two generals on campaign. They have an objective (a hill) which they want to capture. If they simultaneously march on the objective they are assured of success. If only one marches, he will be annihilated.*

*The generals are encamped only a short distance apart, but due to technical difficulties, they can communicate only via runners. These messengers have a flaw, every time they venture out of camp they stand some chance of getting lost (they are not very smart.)*

*The problem is to find some protocol which allows the generals to march together even though some messengers get lost.*

Gray provides a short proof that no such protocol is possible, and then introduces the two-phase commit protocol as a way around the problem. You can see the fairly obvious equivalence to the Dr. Ecco problem.

The three scenarios share the same basic features. There are two parties (generals or gangsters) and there are messengers sent between the parties, using either smart pigeons or not so smart people. The equivalence to distributed systems problems in computer science is that: the generals/gangsters are processes, and the messages are sent via some inter-process communication method, we’ll say a network. Can you coordinate a distributed system by messages and confirmations?

Now suppose there are more than two generals, in fact some arbitrarily large number. Further imagine that one or more of the communicating generals might be duplicitous. In other words, instead of fruitlessly trying to reach some final agreement, one or more of the generals might deliberately fail to respond or even lie. Finally, assume that all of the parties are communicating in pairs– in other words, there’s no “broadcasting” of a message to all of the other parties at once, and no “common knowledge” (say, signal fires). The objective is no longer to get *all* parties to agree, but rather to see if agreement can be reached among the parties who are acting in good faith.

In 1980 Marshall Pease, Robert Shostak and Leslie Lamport wrote a paper about this scenario with the straightforward title “Reaching Agreement in the Presence of Faults”. They describe it thusly:

*Our results are formulated using the notion of* interactive consistency*, which we define as follows: Consider a set of n isolated processors, of which it is known that no more than m are faulty. It is not known, however, which processors are faulty. Suppose that the processors can communicate only by means of two-party messages. The communication medium is presumed to be fail-safe and of negligible delay. The sender of a message, moreover, Is always identifiable by the receiver. Suppose also that each processor p has some private value of information Vp (such as its clock value or its reading of some sensor). The question is whether for given m, n \_> 0, it is possible to devise an algorithm based on an exchange of messages that will allow each nonfaulty processor/) to compute a vector of values with an element for each of the n processors, such that*

1. *the nonfaulty processors compute exactly the same vector;*
2. *the element of this vector corresponding to a given nonfaulty processor is the private value of that processor.*

No generals, no gangsters, no pigeons. There are just processes, some faulty and some non-faulty, communicating via two-party messages. Even the network is reliable. Boooo-ring.

Still, there are some interesting results. They first talk about the single fault case. The setup here is that each party has a value for some variable. In the first step each processor sends their value to all of the other parties in two-party exchanges. In the second step, the processors exchange the values they got from the other processors. Keep in mind that a processor can be faulty, which could mean big fat liar or non-responsive. The hope is that a non-faulty processor will be able to determine the correct value from the complete set of responses.

Intuitively, you might think it would suffice if a majority of the processors declare the same value. But, nope. It turns out that in this setup with a single fault, they show that it would require 4 total processors (i.e. 3 non-faulty) to get a consistent result. In general, if there are N total processors and M faulty processors then to guarantee a consensus value N must be greater than or equal to (3M + 1), and this would require M+1 rounds of information exchange.

A second result is that if you prevent the faulty processors from *lying* about the values that other processors have shared with it, then a simple majority *will* work. They propose something like a message digest, where attempting to alter the value sent by other processors would be detectable.

A couple of years later the same trio published another paper called The Byzantine Generals Problem. This covers a lot of the same ground as their previous paper, but check out this passage from the abstract:

*Reliable computer systems must handle malfunctioning components that give conflicting information to different parts of the system. This situation can be expressed abstractly in terms of a group of generals of the Byzantine army camped with their troops around an enemy city. Communicating only by messenger, the generals must agree upon a common battle plan. However, one or more of them may be traitors who will try to confuse the others. The problem is to find an algorithm to ensure that the loyal generals will reach agreement.*

Oh, hell yeah. Generals, the Byzantine army, battle plans, traitors. Now we’re talking.

In a [prolog to the paper](https://www.microsoft.com/en-us/research/publication/byzantine-generals-problem/) on the Microsoft Research site, Lamport says that he thinks Djikstra’s Dining Philosophers Problem received more attention than it deserved, mostly because it had a cute name, and so “The main reason for writing this paper was to assign the new name to the problem.” Lamport also notes that the use of *Byzantine* generals was intentional, to avoid potentially offending any existing nationality (he says that the two generals problem above is often called “The Chinese Generals Problem”, though I have not found any written reference to that between Gray’s paper and this one).

Sidebar: definitionsof “Byzantine”: <https://www.merriam-webster.com/dictionary/Byzantine>

- Related to Byzantium (Istanbul during Eastern Roman Empire period which ends at the fall of Constantinople)
- Architectural style of this time period and location
- Devious or surreptitious manner of operation
- Intricately involved, labrynthine

From Miriam Webster definition page:

Today, the city that lies on the Bosporus Strait in Turkey is named Istanbul, but it was once known as Constantinople (a name given to it when it became the capital of the Byzantine Empire, aka the eastern half of the Roman Empire), and in ancient times, it was called Byzantium. Its history is legendary—filled with mystics, wars, and political infighting—and over time the word Byzantine (from the Late Latin word Byzantinus, the name for a native of Byzantium) became synonymous in English with anything characteristic of the city or empire, from architecture to intrigue. The figurative sense used to describe that which is intricately involved and not easily understood first appeared in the early 20th century.

They do add some new material to the paper so it’s not just like when Gus Van Sant did a shot-for-shot remake of *Psycho*. For one, they state the problem in a slightly different way:

*Byzantine Generals Problem. A commanding general must send an order to his n - 1 lieutenant generals such that*

*IC1. All loyal lieutenants obey the same order.*

*IC2. If the commanding general is loyal, then every loyal lieutenant obeys the order he sends.*

Notice that the problem is stated now in terms of one general sending messages to lieutenant generals.

Let’s look at the setup in a bit more detail. There’s a bunch of generals and they have to decide collectively whether to attack or retreat. They exchange messages in pairs (General A sends a message to B and a separate message to C, and so on) and these are *oral messages*, which in this context implies that there’s no way to verify that A sends the same message to B and C. All of the N generals will create a set of responses, their own plus one for each of the other N-1 generals. To extend the metaphor assume they each have an aide-de-camp with a notebook who’s recording the responses.

The generals need to have a predetermined algorithm for deciding what to do with the information so that these two conditions are guaranteed:

- All loyal generals take the same action
- A small number of traitors can’t make the loyal generals choose a bad plan

Let’s say there are 10 generals, 3 of whom are disloyal. All of the generals send messages to the other generals saying “Attack” or “Retreat”. In the case of loyal generals, this is their assessment of the best action to take, while with disloyal generals it might be the opposite of what they think (but, also, they might send “Attack” to General D and “Retreat” to General H).

Suppose the algorithm is “use the majority action”. All the loyal generals look at the opposing army and say, “Nope, put me down for ‘Retreat’”, and the disloyal generals say “Oh, they’ll destroy us so I'm gonna say ‘Attack’, then head for the hills”. The vote is then 7-3 for Retreat and all is good. But if the loyal generals split 4 Retreats and 3 Attacks, then the disloyal generals would change the result. Arguably that’s not a bad plan since the loyal generals are nearly split on the right action, and they will at least all take the same action. On the other hand if the disloyal generals send different messages to the loyal generals, a situation could arise where the loyal generals take different actions.

So the authors add the following conditions to the algorithm:

1. Every loyal general must get the same set of answers
2. If a general is loyal then their response must be used by every other loyal general

They further note that Condition 1 is the same as saying “Loyal Generals B and C will always use the same value for General A, whether A is loyal or not”. Condition 2 could be thought of as “If General A is loyal, then every loyal General will use A’s response”. Since the conditions now depend only on the responses of General A, this leads to the new formulation of the problem where a General is sending messages to lieutenant Generals.

The paper has a new proof of the N >= (3M + 1) result in which the Byzantine Generals simulate Albanian Generals (It makes more sense in the context of the paper). They also prove some results about what happens when some of the parties can’t communicate directly but only through intermediaries. They also use the metaphor to clarify some of the details. For instance, they call the basic messages “oral messages” meaning that they are sent by messengers who simply repeat the general A’s message to general B. In contrast, “signed messages” are written down, signed by the generals in an unforgeable way, and the message can’t be altered without detection.

They also provide an algorithm for the process that works for the case where N >= (3M + 1), which they called the OM(M) algorithm (OM is “oral message”, M is both the number of traitors and the number of rounds of message exchange). The essence of the idea here is that some general starts off as the commanding general and sends messages to all of the lieutenants as described above. But then, each lieutenant acts like the commander and relays the first general’s value to all of the other generals except the original commander. The result is that every lieutenant gets multiple copies of the original message and so can determine a value by majority vote. This continues for M rounds so that every general sees multiple versions of the message for the other generals. Keep in mind though that this only works if N >= (3M + 1). There’s a Python version of this algorithm implemented [here](https://github.com/JVerwolf/byzantine_generals/blob/master/byzantine_generals.py).

Since this paper, the type of faults involving a server supplying erroneous information or acting maliciously have been called *Byzantine faults*. A search on Google Scholar for “the Byzantine Generals Problem” returns 97000 results and “Byzantine fault tolerance” returns another 64k. This has been an extremely well-researched problem over the last 40 years. So it’s possible that Lamport was right that the idea would get more attention with a catchy name. On the other hand the idea of Byzantine fault tolerance would probably have become a big deal in any case thanks to the Internet, though it’d be a bummer if it had a less cool name.

In 1999 Miguel Castro, a student of Barbara Liskov, published a paper called *Practical Byzantine Fault Tolerance*, which has been cited over 6000 times according to Google Scholar. This paper develops a method that’s both implementable and performant for doing replication in the presence of faulty or malicious servers. As the abstract says:

*This paper describes a new replication algorithm that is able to tolerate Byzantine faults. We believe that Byzantine fault-tolerant algorithms will be increasingly important in the future because malicious attacks and software errors are increasingly common and can cause faulty nodes to exhibit arbitrary behavior.*

Hard to argue with that. Byzantine fault tolerance (BFT) is an important property for Internet-based distributed systems, and also in some physical systems like the sensors in aircraft, and even air-traffic control.

The main innovations of Practical Byzantine Fault Tolerance (PBFT) are that it works in an asynchronous system, and that it uses message authentication codes (MAC) rather than full digital signatures in most situations. Here “asynchronous” means that it doesn’t rely on getting message responses within some fixed delay period. Although it was first published in 1999, PBFT is still the basis of many real distributed systems.

Only 3 years after the Byzantine Generals paper, Fischer, Lynch, and Paterson (FLP) would prove that consensus can’t be guaranteed in a distributed system with only a single fault. PBFT, which seems to offer consensus with nearly a third of the participants being faulty, seems to contradict this. However, there’s a line in the PBFT paper that says:

*The algorithm does not rely on synchrony to provide safety. Therefore, it must rely on synchrony to provide liveness; otherwise it could be used to implement consensus in an asynchronous system, which is not possible*

This refers to the FLP result. Castro and Liskov introduce a weak synchrony condition to guarantee liveness, or that clients will eventually get a response. They say *“delay(t) does not grow faster than t indefinitely. Here, delay(t) is the time between the moment when a message is sent for the first time and the moment when it is received by its destination*”. Not sure what that means, but I interpret it to mean that the time you wait for a response can’t grow arbitrarily large, because then you’re back to the problem in FLP where you can’t distinguish between a non-responsive server and a delayed message.

The BFT concept hit new highs of recognition in the twenty-teens due to a new meme vector: the blockchain. The idea of money based on a peer-to-peer distributed system is practically the worst-case scenario for BFT. For one, the whole point is to arrive at consensus on a sequence of transactions. Now though the incentives inspire people to be not just malicious but collectively malicious, and by definition there’s no owner of the network who can control who can participate. Put another way, the traitorous generals are now colluding. The intent of things like proof-of-work is specifically to make it difficult and expensive to defeat the consensus (but not impossible).

- [Some constraints and tradeoffs in the design of network communications | Proceedings of the fifth ACM symposium on Operating systems principles](https://dl.acm.org/doi/10.1145/800213.806523)
  + Probably the original statement of the Two Generals problem, but described as communication between a group of gangsters
- [Notes on Data Base Operating Systems](https://dl.acm.org/doi/10.5555/647433.723863)
  + Can’t really get this doc on-line, but supposedly Gray gave it the name “Two Generals Paradox”
- [Reaching Agreement in the Presence of Faults | Journal of the ACM](https://dl.acm.org/doi/10.1145/322186.322188)
- [The Byzantine Generals Problem - Microsoft Research](https://www.microsoft.com/en-us/research/publication/byzantine-generals-problem/)
- [GitHub - JVerwolf/byzantine\_generals: An implementation of Leslie Lamport's OM algorithm for the Byzantine Generals Problem](https://github.com/JVerwolf/byzantine_generals)
- [The Byzantine Generals Strike Again](http://infolab.stanford.edu/pub/cstr/reports/cs/tr/81/846/CS-TR-81-846.pdf)
- [The Chinese Generals Problem](https://citeseerx.ist.psu.edu/document?repid=rep1&type=pdf&doi=f8c0b8d975cb12d4cd80190f2a4d7d327e69e175)
- [SIFT: Design and Analysis of a Fault-Tolerant Computer for Aircraft Control](https://citeseerx.ist.psu.edu/document?repid=rep1&type=pdf&doi=f7fbc22d361fb9e4b53f2168a9d4968956e5ce16)
- [The Generals – Dean Eigenmann](https://dean.eigenmann.me/blog/2020/05/06/generals/)
- [Practical Byzantine fault tolerance | Proceedings of the third symposium on Operating systems design and implementation](https://dl.acm.org/doi/10.5555/296806.296824)