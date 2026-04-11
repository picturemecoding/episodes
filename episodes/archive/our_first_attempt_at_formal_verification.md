# Into the Well of Formal Verifications

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

Erik music: (minimalism) Philip Glass, Third Coast Percussion *Perpetulum* (an earlier record, which I loved, was *Paddle to the Sea*). Vaguely soundtrackish, but so layered and complex that you “have to lean-in to listen” (something Steve Reich said about them). This year their *Currents Volume 3* is also great. They’re from Chicago and they play marimbas and other percussion instruments, and I think everyone should check out this group immediately. If you’re confused about the world and you need something to wash over you and tickle your brainwaves, this music will get the job done. Go out and listen to Third Coast Percussion as soon as possible.

Mike music: Porridge Radio - *Clouds in the Sky They Will Always Be There For Me*

UK indie rock band, from Brighton. Interesting, very personal and introspective lyrics. Also, on the Secretly Canadian label, which put out most of Jason Molina’s albums.

—

The thing my heart ardently desires: a correct program. If the ticket says “when the account holder transfers negative dollars, I want our program to drain their account,” then the ticket may be wrong, but dammit I want to *correctly implement what the ticket says*!

This is our first foray into formal verification as a topic on the podcast. It’s always confusing when I hear about these things, because I feel like I’ve *heard* about loads of them over the years but I’ve virtually never met anyone in software engineering environments who uses them.

This was inspired by two things: first I have been reading this fascinating and sometimes funny book by Leslie Lamport called *A Science of Concurrent Programs*. Secondarily, the Amazon Science [page on “automated reasoning” as an area](https://www.amazon.science/research-areas/automated-reasoning) of research. What is “[automated reasoning](https://aws.amazon.com/what-is/automated-reasoning/)”?

Automated reasoning is a specific discipline of artificial intelligence (AI) that applies logical deduction to computer systems…

Automated reasoning differs from other AI technologies, such as natural language processing (NLP), which focuses on training computers to understand written or verbal speeches. Instead, automated reasoning uses logical models and proofs to reason about the possible behaviors of a system or program, including states it can or will never reach.

This sounds promising! One of their papers listed here is titled: “[Model checking distributed protocols in must](https://www.amazon.science/publications/model-checking-distributed-protocols-in-must)” and I was curious about this so I skimmed it and found the following introduction:

Despite significant research advances in the past years, formal methods are yet to be integrated in the development process of distributed systems. A major hindrance in their adoption is that existing techniques fail to adequately balance the conflicting desiderata of researchers and programmers: researchers typically design powerful (yet expensive) reasoning techniques for programs written in domain-specific languages, while programmers prefer mainstream programming languages, and do not tolerate non-scalable tools that interfere with their development. As such, while domain-specific languages for distributed systems offer abstract modeling and systematic exploration for finite-state abstractions, they do not integrate well with the rest of the software engineering process. On the other hand, tools for systematic concurrency testing built on mainstream languages do not provide adequate interfaces for abstract modeling (e.g., no support for data non-determinism, multiple communication primitives, etc), and/or do not provide any verification guarantees.

Is this not the exact same thing that everyone always says about formal verification for working software engineers?

## Questions to answer for the audience

- What is formal verification?
  + Very broadly, using some formal (mathematical, logical) method to verify that a program correctly implements a formal specification.
- History
  + Does it start with lambda calculus?
  + Hoare Logic?
  + The first time i came across these things was w/r/t hardware design (eg Verilog)
  + CSP(1978-1985), Verilog (1984), Coq (1989), TLA+ (1999), PlusCal (2009)
  + Curry-Howard (Wadler’s [famous StrangeLoop talk](https://www.youtube.com/watch?v=IOiZatlZtGU) and his [paper](https://homepages.inf.ed.ac.uk/wadler/papers/propositions-as-types/propositions-as-types.pdf))
  + See also Lamport’s Comments below
- How does it typically look or how is it typically run (as opposed to programs in traditional programming languages)?
- What formal verification languages and tools are out there?
- How can we *group* formal verification languages and tools?
  + DSLs
  + Things with their own client/toolbox to run the things (TLA+...?)
  + Math ones (Lean…?)
  + Distributed Systems ones…
  + There seem to be 3 broad categories of methods:
    - Model checkers
    - Proof assistants
    - SAT/SMT
- Does static analysis or undefined-behavior detection (like miri in rust) count?
- When (the hell) am I ever going to use this at work? Alternatively: why *can’t we all use this* at work*?*

### Formal Verification Languages Enumerated…

- Proof Assistants
  + Agda
  + Coq
  + Idris
  + Isabelle
  + Lean
  + Full list: <https://en.wikipedia.org/wiki/Proof_assistant>
- Model Checkers
  + TLA+, PlusCal
  + PAT
  + PRISM
  + SPIN
  + FizBee
  + P lang (sponsored by MS) (also a programming language!)
  + Cloudflare Topaz: https://blog.cloudflare.com/topaz-policy-engine-design/
  + Wikipedia list: https://en.wikipedia.org/wiki/List\_of\_model\_checking\_tools
- SMT
  + DPT
  + OpenCog
  + OpenSMT
  + Z3
- Programming Languages (not verification tools, but help to build correct programs)
  + Idris (dependent types)
  + Haskell (no dependent types but compilable types as proofs)
  + OCaml
- Not sure
  + [Dafny](https://dafny.org/) (i think it uses SMT)
  + Alloy (was a model checker, now claims to be SAT?)
- New Things
  + Amazon Must paper

### TLA+

From *A Science of Concurrent Programs*, on the origins of TLA+:

Programming is not just coding. It requires thinking before we code. Writing algorithms taught me that there are two things we need to decide before writing and debugging the code: what the program should do and how the program should do it. Most programmers think that the code itself adequately describes “how the program should do it”, but I learned that we need a higher-level, more abstract description of what the program does…

Engineers who build complex systems usually recognize the need for describing what their programs do in a simpler, more abstract way than with code. I decided that abstract programs written in math provided such a way for describing the aspects of a system that involve concurrency. By about 1995, I had designed a complete language called TLA+ that engineers could use to write abstract programs in TLA.

### Hillel Wayne [Let's Prove Leftpad](https://www.hillelwayne.com/post/lpl/)

Repo: <https://github.com/hwayne/lets-prove-leftpad>)

From the above in the Idris example:

In a dependently typed language, theorems are just a certain way of looking at types, and proofs of theorems are just programs with those types. This means that we can prove a program is correct just by giving it a precise-enough type.

### Detour Into Dependent Types

From *A Science of Concurrent Programs*:

I believe types are wonderful in a coding language, and I wouldn’t want to write code in an untyped language. However, I have found the kind of types provided by coding languages to be unsuitable for representing abstract programs. For example, type correctness for Euclid’s algorithm means that the values of the variables x and y are positive integers. The simple type systems of most coding languages don’t allow a type like positive integer. Moreover, those type systems are overly restrictive, disallowing some reasonable expressions.

There are type systems that allow the type positive integer and are not restrictive. They are needed to formalize the kind of math that mathematicians do, so their proofs can be checked by computer. However, those type systems are so complicated that I never tried to learn them. I didn’t have to because I realized that they are not needed for the math used to describe and reason about abstract programs. Instead, type correctness can be treated as a simple invariance property of programs.

### Curry-Howard / Propositions as Types

This is a sidebar: mention it. Don’t go too deep! Save for a future episode. Wadler’s [famous StrangeLoop talk](https://www.youtube.com/watch?v=IOiZatlZtGU) and his [paper](https://homepages.inf.ed.ac.uk/wadler/papers/propositions-as-types/propositions-as-types.pdf)

### [P language](https://www.microsoft.com/en-us/research/blog/p-programming-language-asynchrony/) (see also https://github.com/p-org/P):

To address the challenges of asynchronous computation, we have developed P(opens in new tab), a programming language for modeling and specifying protocols in asynchronous event-driven applications. This project is a collaborative effort between Microsoft researchers and engineers, and academic researchers at the University of California, Berkeley and Imperial College in London.

…

Unlike TLA+ and SPIN, a P program can also be compiled into executable C code.

…

The programming model in P is based on concurrently executing state machines communicating via events, with each event accompanied by a typed payload value. A memory management system based on linear typing and unique pointers provides safe memory management and data-race-free concurrent execution. In this respect, P is similar to modern systems programming languages such as Rust(opens in new tab).

### Cloudflare and Topaz

[Published this week](https://blog.cloudflare.com/topaz-policy-engine-design/)!

Over the last year, Cloudflare has begun formally verifying the correctness of our internal DNS addressing behavior — the logic that determines which IP address a DNS query receives when it hits our authoritative nameserver. This means that for every possible DNS query for a proxied domain we could receive, we try to mathematically prove properties about our DNS addressing behavior, even when different systems (owned by different teams) at Cloudflare have contradictory views on which IP addresses should be returned.

To achieve this, we formally verify the programs — written in a custom Lisp-like programming language — that our nameserver executes when it receives a DNS query. These programs determine which IP addresses to return. Whenever an engineer changes one of these programs, we run all the programs through our custom model checker (written in Racket + Rosette) to check for certain bugs (e.g., one program overshadowing another) before the programs are deployed.

Our formal verifier runs in production today, and is part of a larger addressing system called Topaz. In fact, it’s likely you’ve made a DNS query today that triggered a formally verified Topaz program.

### See also:

- “[Software Model Checking](https://people.mpi-sws.org/~rupak/Papers/SoftwareModelChecking.pdf)” by RANJIT JHALA University of California, San Diego, RUPAK MAJUMDAR, University of California, Los Angeles

## Links

- Lamport’s [A Science of Concurrent Programs](https://lamport.azurewebsites.net/tla/science.pdf) (pdf)
- [Dafny](https://dafny.org/)
- [Cloudflare’s Formally Verified DNS](https://blog.cloudflare.com/topaz-policy-engine-design/)
- [P language](https://github.com/p-org/P)
- Wadler’s [Propositions as Types paper](https://homepages.inf.ed.ac.uk/wadler/papers/propositions-as-types/propositions-as-types.pdf)
- Hillel Wayne’s “[Let’s Prove Leftpad](https://www.hillelwayne.com/post/lpl/)” and [repo](https://github.com/hwayne/lets-prove-leftpad)
- Amazon Science [page on “automated reasoning” as an area](https://www.amazon.science/research-areas/automated-reasoning)
- Amazon paper “[Model checking distributed protocols in Must](https://www.amazon.science/publications/model-checking-distributed-protocols-in-must)”