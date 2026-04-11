# The Turing Test

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

### Music

Erik: I keep listening to the Jeff Tweedy album. It’s too long but I can’t figure out what I’d cut from it. I like some of the stuff a lot more than I feel like I should. Like the song “Feel free”: I love that song? Why do I love that song? Or “New Orleans”.

Mike: *The Spiritual Sound* - Agriculture. Disclaimer: i know one of the people in this band. One of the vocalists was in a band with my son during their jr high/high school years. That said, this is one of my favorite metal albums in a while, at least since last year’s Blood Incantation album. Its flavor is black metal, so one’s feelings about it might depend on one’s tolerance for black metal-style extreme vocals. It’s not straight black metal though (the band calls it “ecstatic” black metal). The closest comparison i could come up with is maybe Deafheaven.

## 1950: “Computing Machinery and Intelligence”

Seventy five years ago Alan Turing published a paper entitled Computing Machinery and Intelligence, in which he poses the question: Can machines think? He proposes what he calls “The Imitation Game” as a test of whether or not machines can think, which we now also refer to as The Turing Test.

The Turing Test is often posed in two casual but somewhat misleading ways. The first is that a human interrogator is communicating with two agents, one of whom is human and the other of whom is a machine. The goal is for the interrogator to figure out which is which. The second informal description is what Turing himself calls *viva voce*, where the human interrogator is communicating with only a single agent, with the goal of determining if it is human or machine.

The actual statement of the Imitation Game is a bit more subtle.

*It is played with three people, a man (A), a woman (B), and an interrogator (C) who may be of either sex. The interrogator stays in a room apart from the other two. The object of the game for the interrogator is to determine which of the other two is the man and which is the woman.*

- A plays to *deceive*
- B plays to *assist* the interrogator to the answer
- Answers are type-written from separate rooms

(The last one seems like an implementation detail?)

We now ask the question: “What will happen when a machine takes the part of A in this game?” Will the interrogator decide wrongly as often when the game is played like this as he does when the game is played between a man and a woman? These questions replace our original, “can machines think?”

*Mike: To me what’s interesting about this setup is that it requires a level of deceit from the AI. This is both inherently interesting (implying, maybe, that deceit is a “human” quality), but also answers a question that’s always bothered me about the TT: how do you keep the interrogator from detecting the AI by asking questions that a human could not answer. For example, i asked Gemini “What’s the first prime number larger than 2 billion?” It said 2,000,000,011, in about 4 seconds. Turing mentions “Multiply 3540675445 by 7076345687”, which Gemini did immediately, but i was also able to get the answer with Python in a few seconds. A deceitful AI can just say “I don’t know”*

From the Jones and Bergen paper “Large Language Models Pass the Turing Test”:

At its core, the Turing test is a measure of substitutability: whether a system can stand-in for a real person without an interlocutor noticing the difference. Machines that can imitate people’s conversation so well as to replace them could automate jobs and disrupt society by replacing the social and economic functions of real people. More narrowly, the Turing test is an exacting measure of a model’s ability to deceive people: to bring them to have a false belief that the model is a real person.

They actually say that the three-party Turing test is harder than just having someone chat directly with a computer, because you get a direct comparison to human responses.

Seems worth it to mention that this is necessarily a game *in language* because it levels the playing field for machines. You wouldn’t want a test which is like, “build a restaurant and serve food at it.”

Section 2

Turing refines the core question of the paper using this test. He focuses a lot on the “best question” to ask, saying “is the new question a worthy one to investigate?”

Section 3

He addresses an objection that “machines don’t think like people” with the answer: if a machine plays this game well enough, we can leave this question behind.

Here is where Turing states his belief in the paper plainly: eventually digital computers will exist which can pass the game.

We could even try it with the computers we have now he says, but he answers that like this:

The short answer is that we are not asking whether *all* digital computers would do well in the game nor whether the computers at present available would do well, but whether there are *imaginable* computers which would do well.

This is the question he wants to replace “can machines think?” with.

There’s a lot of metacommentary in this paper around *finding the best question*. In addition, he spends a lot of time refining his terms, such as what we mean by the word “machine”. These traits are why it reminds me of a work of philosophy, actually.

I think he should have started with this question if he really wanted to pursue it. Starting with the question “can machines think?” when he explicitly thinks it’s a bad question is problematic for his readers.

He even says, later on, in section 6:

The original question “can machines think?” I believe to be too meaningless to deserve discussion.

Why is that the first line in the paper then?!

There is also this:

We may now consider again the point raised at the end of §3. It was suggested tentatively that the question, "Can machines think?" should be replaced by "Are there imaginable digital computers which would do well in the imitation game?" If we wish we can make this superficially more general and ask "Are there discrete-state machines which would do well?" But in view of the universality property we see that either of these questions is equivalent to this, "Let us fix our attention on one particular digital computer C. Is it true that by modifying this computer to have an adequate storage, suitably increasing its speed of action, and providing it with an appropriate programme, C can be made to play satisfactorily the part of A in the imitation game, the part of B being taken by a man?"

Section 4

The reader must accept it as a fact that digital computers can be constructed, and indeed have been constructed, according to the principles we have described, an that they can in fact mimic the actions of a human computer very closely.

It’s important to Turing here that digital computers are *mechanical* and he gives the examples of Babbage’s Analytical Engine. He says we should ignore electrical signalling as a superstition when comparing brains and digital computers. Computers are mechanical, so this analogy misleads. He says

The feature of using electricity is thus seen to be only a very superficial similarity. If we wish to find such similarities we should look rather for mathematical analogies of function.

Thus, he believes a mechanical instrument can be constructed which passes for a human playing the imitation game. That’s a pretty weird idea!

Section 6: Contrary Views

Here’s the most stunning line in the paper for me, not quite halfway through it:

Nevertheless, I believe that at the end of the century, the use of words and general educated opinion will have altered so much that one will be able to speak of machines thinking without expecting to be contradicted (p.9)

Various Objections offered:

- Theological
- Head in the Sand
- Mathematical Objection (refers to Godel’s Impossibility Result here)
- Argument from Consciousness
- Arguments from various disabilities (“but a computer can’t do X or Y!”)
- Ada Lovelace: computers only do what they’re programming to do
- Argument from "informality of behavior”: humans adapt to situations; no rules encode their behavior
- Argument from ESP/telepathy(?) (We don’t know yet if there are other senses involved…?)

#### Mathematical

On the MathematicalObjection (and Godel’s Impossibility Result here): he says people give wrong answers too. “When we know a computer’s answer to a question is wrong, it gives us a feeling of superiority…” But when we’re superior, we’re not superior over *all machines*, just that one. They’re just going to get better and better! (Sounding very Sam Altman here, implying they will scale to the problems).

#### Consciousness

On the Consciousnessobjection: digital computers can’t *intend* and they can’t *mean*. Turing says,

This argument appears to be a denial of the validity of our test. According to the most extreme form of this view, the only way by which one could be sure that a machine thinks is to be the machine and feel oneself thinking.

He ultimately says, we don’t have to solve the mystery of consciousness before we can answer the question of this paper: can a machine be constructed which can pass this test.

#### Disabilities

On the Disabilities objection: he says sometimes people claim computers can’t make mistakes (because of their procedural, mechanical nature; it’s like asking, “can a sewing machine make a mistake?”)

He raises this interesting point about how these criticisms depend on confusing two kinds of mistakes: “errors of functioning” and “errors of conclusion.”

This distinction he makes immediately reminded me of the kinds of discussions we see around the utility of fancy type systems: they do not prevent “errors of conclusion.”

I watched [a podcast](https://newsletter.pragmaticengineer.com/p/python-go-rust-typescript-and-ai) the other day where Armin Ronacher was being interviewed by Gergely Orosz, who has a newsletter and a podcast called *The Pragmatic Engineer*.

Armin was the first engineer at Sentry, which is a software company that provides error reporting and alerting. Sentry is a successful company with customers all over the world, processing huge amounts of logs, errors, alerts, etc.

Anyway, Armin said in the podcast that the volume of errors *is noticeably different* in different language families: that Javascript errors vastly outstrip errors in other languages. He said they would see massive numbers of Javascript errors, but these were often ignorable, not very meaningful.

He compared this to game engines in C++ which typically had very few errors.

But then he said that, in response to a question about static typing, I think, that when frontend apps switched to Typescript, which he credits as being a good language, the volume of errors for those Javascript projects *did not reduce*: they’re still spewing out well beyond the share.

Anyway, the thing you hear sometimes in the static vs dynamic typing argument is that a fancy type system won’t stop you from dividing by zero without doing a massive amount of work to achieve that type safety (Lamport’s line). It won’t stop *errors of conclusion*, I think, is what people are saying.

Errors of function vs errors of conclusion.

#### Ada Lovelace

For the Lovelaceobjection, that a computer can’t do anything new, Turing seems to be compelled by this. But then he posits, “how do we know that humans are truly doing something new?”

#### Informality and Adaptability of Human Behavior

Finally, the informalityobjection is pretty interesting. He says that we could not produce a program which would determine what a human would do in allscenarios. Thus, humans cannot be machines.

Here is the place where he gets closest to talking about agency, autonomy, and independence. Those are the key ideas, I think. If there is a program for humans, it potentially relates to the free will question: are we programmed by our biology? Are we compelled to act in specific ways in all situations?

To “win” at the Imitation Game, do you need *agency*? Do you need intention? This is where Searle comes in later.

Final Section

Computers will be able to learn, he says, which is going to shock some people:

The idea of a learning machine may appear paradoxical to some readers. How can the rules of operation of hte machine change? They should describe completely how the machine will react whatever its history might be, whatever changes it might undergo. The rules are thus quite time-invariant.

An important feature of a learning machine is that its teacher will often be very largely ignorant of quite what is going on inside. (!)

This sounds like neural networks! It sounds like LLMs!

Comment: this paper seems to include a lot of “I believe this will come to pass…” which is funny for a mathematician like Turing in a Philosophy paper…

## Attempts to Pass the Turing Test

- 1966: Eliza fooled some people
- 2011: IBM’s Watson on Jeopardy?
- 2014: Eugene Goostman “A 13-year-old Ukrainian child” (from [an article](https://www.aiplusinfo.com/has-any-ai-passed-the-turing-test/))

Eugene Goostman is a chatbot that, in 2014, claimed to have passed the Turing Test by fooling 33% of human judges into believing it was a 13-year-old Ukrainian boy. The event was organized by the University of Reading, and the results were initially hailed as a landmark achievement.

The claim was subsequently met with skepticism. Critics pointed out that the chatbot was strategically designed to be a 13-year-old non-native English speaker, thereby lowering expectations for the quality and depth of its responses. This led to questions about whether the Turing Test conditions were manipulated to give the AI an advantage.

- 2024 paper, previously mentioned: “Large Language Models Pass the Turing Test” by Cameron Jones and Benjamin Bergen (from the Cognitive Science department at UCSD!)

From the abstract:

We evaluated 4 systems (ELIZA, GPT-4o, LLaMa-3.1-405B, and GPT-4.5) in

two randomised, controlled, and pre-registered Turing tests on independent pop-

ulations. Participants had 5 minute conversations simultaneously with another

human participant and one of these systems before judging which conversational

partner they thought was human. When prompted to adopt a humanlike persona,

GPT-4.5 was judged to be the human 73% of the time: significantly more often

than interrogators selected the real human participant. LLaMa-3.1, with the same

prompt, was judged to be the human 56% of the time—not significantly more or

less often than the humans they were being compared to—while baseline models

(ELIZA and GPT-4o) achieved win rates significantly below chance (23% and 21%

respectively). The results constitute the first empirical evidence that any artificial

system passes a standard three-party Turing test. The results have implications for

debates about what kind of intelligence is exhibited by Large Language Models

(LLMs), and the social and economic impacts these systems are likely to have.

So GPT 4.5 is “more than human”! It has passed!

## Critiques of the Turing Test

Jones and Bergen in the “Large Language Models Pass the Turing Test” paper write:

Turing’s article “has unquestionably generated more commentary and controversy than any other article in the field of artificial intelligence” (French, 2000, p. 116). Turing originally proposed the test as a very general measure of intelligence, in that the machine would have to be able to imitate human behaviour on “almost any one of the fields of human endeavour” (Turing, 1950, p. 436) that are available in natural language. However, others have argued that the test might be too easy—because human judges are fallible (Gunderson, 1964; Hayes and Ford, 1995)—or too hard in that the machine must deceive while humans need only be honest (French, 2000; Saygin et al., 2000). (from [link](https://arxiv.org/pdf/2503.23674))

Searle’s 1980 paper, “[Minds, Brains, and Programs](https://home.csulb.edu/~cwallis/382/readings/482/searle.minds.brains.programs.bbs.1980.pdf)” is where the Chinese Room comes from. He says in the abstract that he’s going to talk about *intentionality*. That brains have intentionality. That starting a program is not the same as endowing it with intentionality.

He refers to any attempt to create intentionality as “strong AI”. He is specifically responding to the work of Roger Schank at Yale in the 70s, where they would input these stories into computer programs and then ask a question: “a man goes into a restaurant and orders a hamburger. He enjoys it and he leaves a large tip. Did he eat the hamburger?”

He rejects the idea that computers have cognitive states (which he says is required for human cognition). You can see how the Chinese Room comes out of this: there are no cognitive states involved in applying rules to match symbols. It’s weird because “cognitive states” are kind of analogous to the discrete states Turing is talking about. Searle is kind of arguing that cognitive states are *different* from the states of a discrete state machine.

This ultimately gets at the question of what brains are: they receive inputs/stimuli from their environment, they process these stimuli, they come up with a response (statement or action or internal action?). Do brains compute?

How can we bridge the gap between computation and *intentionality* or *agency*? I look at a bee foraging for nectar: it’s part of a hive. It has autonomy but seems to have something like the least possible agency of the animal world. Its behavior looks encoded or determined. But bees decide to sting sometimes! I got stung on the neck by one in the pool the other day. Did it require cognitive states to sting me? Can we imagine a program determining that it should sting me on the back of the neck? Can it be argued that the sting happens *without* intentionality?

Maybe a virus is a better analogy: it looks like a program, not an animal. It cannot have intentionality, and yet it does *intentionally propagate itself.* There’s like one program encoded in the virus: to propagate itself?

Searle offers a bunch of replies to his Chinese Room analogy. The one that seems closest to me to responding to the Turing test is the “other minds” reply, where people say,

How do you know that other people understand Chinese or anything else? Only by their behavior. Now the computer can pass the behavioral tests as well as they can (in principle), so if you are going to attribute cognition to other people you must in principle also attribute it to computers.

He says this only merits a short reply! It’s not about whether *other people have cognitive states* but *what* we are attributing to them when we *attribute* cognitive states. This is similar to Turing’s consciousness argument: “I can only for sure say the computer *intends* something if I *am the computer* and I *am intending something.”*

## Discussion Wrap-Up

Q: Are you surprised that GPT-4.5 passed the Turing test? That it beats humans at it?

Q: Follow-up: if you repeat this test every year with LLMs, do you think the human evaluators will improve?(!) (Erik: I do: it gets more obvious all the time when I’m seeing the output of an LLM)

Q: Is the Turing Test relevant for us? Substitution for a human *does seem like*, “well, I don’t need to hire humans anymore for that task…” For example, in asking the Schank restaurant-game question: “did the man eat the hamburger?” You wouldn’t need to hire anyone anymore to answer whether the men ate the hamburgers! We can automate that shit!

Q: Humans are so hard-wired to *communicate* that we will cross chasms to do it…

- Is it interesting that a discrete-state machine can imitate human conversationalists?
- Or maybe human sociality means we are willing and eager to talk to *something, anything*? If someone can start building a bridge over any communication gap, we will eagerly complete the bridge. It’s hard to quantify our participation in the deception, in other words?

Q: Does our ability to move and interact with the physical world give us intellectual abilities that can’t be easily reproduced by a brain in a box?

##

## References

[COMPUTING MACHINERY AND INTELLIGENCE](https://courses.cs.umbc.edu/471/papers/turing.pdf)

[Large Language Models Pass the Turing Test](https://arxiv.org/pdf/2503.23674)

[Pragmatic Engineer podcast](https://newsletter.pragmaticengineer.com/p/python-go-rust-typescript-and-ai) interview of Armin Ronacher

<https://en.wikipedia.org/wiki/John_Searle>

“[Minds, Brains, and Programs](https://home.csulb.edu/~cwallis/382/readings/482/searle.minds.brains.programs.bbs.1980.pdf)”