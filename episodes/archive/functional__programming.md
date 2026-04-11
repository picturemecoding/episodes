# Functional Programming

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

Music:

Mike: Slutworld by Slut Intent

Erik: Brad Mehldau Ride Into the Sun, the Elliot Smith record

[E] Mike, today is the 50th anniversary of the release of Scheme, a Lisp based on lambda calculus, the first language with tail call optimization… so I wanted to talk about functional programming today.

[M] About 10 years ago, maybe even 5, it was possible to think that functional programming could become mainstream and supplant object-oriented styles as the main paradigm. But as of January 2026, the only languages in the TIOBE Top 20 that could maybe be considered as functional are Rust and Kotlin. Haskell is 36th, just behind [Visual FoxPro](https://en.wikipedia.org/wiki/Visual_FoxPro). OCaml is in the “Next 50” set, i.e. outside the Top 50.

## What Is It?

Common features

- Functions (duh), and especially composition of functions.
- Static types
- Immutable data
- Lambdas
- Higher-order functions
- Recursion

Less Common Features

- Type inference
- Algebraic data types
- Type classes
- Referential transparency/Purity/Effect tracking
- Parametric polymorphism

### Which Languages are Functional?

- Haskell
- OCaml
- Lisp(s)
- Scala
- Elm
- Clojure
- Typescript ?
- Swift ?
- Rust ?
- Kotlin ?

## A Historical Perspective

- Lambda calc
- LISP
- ISWIM
- APL
- ML

[E]

1. Lambda calculus (Church-Turing)
2. McCarthy and Lisp
3. Backus Turing Lecture 1977 "Can Programming Be Liberated From the von Neumann Style?"
4. ML
5. Djikstra in texas, early 80s
6. Miranda
7. Haskell
   1. Wadler and “Monads for Functional Programming”
   2. Wadler definition: a language based on lambda calculus
8. Then -> the fake functionals: “Functional python”, “functional javascript”, “React”
9. Rust built on Ocaml

### Alonzo Church and Lambda Calculus

[M] Very short discussion here…

### John McCarthy and Lisp

[E]

McCarthy realized that lambda expressions could compute symbolic expressions, and that’s how Lisp was born in 1958. McCarthy wanted a language for doing artificial intelligence, and he was inspired by lambda calculus to make these kinds of algebraic expressions.

“Lisp” stands for “list processing” and it was meant as a way of computing over linked lists. Why lists? Well, McCarthy wrote in his “Lisp prehistory” essay:

Representing sentences by list structure seemed appropriate - it still is - and a list processing language also seemed appropriate for programming the operations involved in deduction - and still is.

He also wanted generalized recursion. He continues in the “Lisp prehistory” essay:

[I wanted to] write recursive function definitions using conditional expressions. The idea of differentiation is obviously recursive, and conditional expressions allowed combining the cases into a single formula.

To use functions as arguments, one needs a notation for functions, and it seemed natural to use the -notation of Church (1941). I didn't understand the rest of his book, so I wasn't tempted to try to implement his more general mechanism for defining functions.

These reasons and others forced him to come up with a new language because what he wanted couldn’t be done in FORTRAN.

### PJ Landin, “The Next 700 Programming Languages” (ISWIM- “If You See What I Mean”)

[M]

ISWIM is thus part. programming language and part program for research. A possible first step in the research program is 1700 doctoral theses called "A Correspondence between x and Church's X-notation. ''~

…

Iswim can be looked on as an attempt to deliver LisP from its eponymous commitment to lists, its reputation for hand-to-mouth storage allocation, the hardware dependent flavor of its pedagogy, its heavy bracketing, and its compromises with tradition.

(c) the thing an expression denotes, i.e., its "value", depends only on the values of its subexpressions, not on other properties of them.

…

When faced with a new notation that borrows the functional appearance of everyday algebra, it is (c) that gives us a test for whether the notation is genuinely functional or merely masquerading.

Program-points are Iswim’s, nearest thing to jumping. [This idea inspired things like continuations and generators]

NOTE: Apparently Landin coined the term “syntactic sugar”, though it doesn’t seem to be in this paper.

[E] That’s interesting because in the history of Scheme that I read they were inspired by Landin in creating Scheme. So Scheme comes out in 1976 and then something happens in 1977 which inspired a whole lot of people to do work in this direction…

### Backus and "Can Programming Be Liberated From the von Neumann Style?", 1977

John Backus is the person who invented Fortran! He gets up to receive his Turing award in 1977 and says the following:

Programming languages appear to be in trouble. Each successive language incorporates, with a little cleaning up, all the features of its predecessors plus a few more. Some languages have manuals exceeding 500 pages; others cram a complex description into shorter manuals by using dense formalisms. The Department of Defense has current plans for a committee-designed language standard that could require a manual as long as 1,000 pages. Each new language claims new and fashionable features, such as strong typing or structured control statements, but the plain fact is that few languages make programming sufficiently cheaper or more reliable to justify the cost of producing and learning to use them.

He moves on to say:

For twenty years programming languages have been steadily progressing toward their present condition of obesity; as a result, the study and invention of programming languages has lost much of its excitement. Instead, it is now the province of those who prefer to work with thick compendia of details rather than wrestle with new ideas. Discussions about programming languages often resemble medieval debates about the number of angels that can dance on the head of a pin instead of exciting contests between fundamentally differing concepts.

And also

The purpose of this article is twofold; first, to suggest that basic defects in the framework of conventional languages make their expressive weakness and their cancerous growthinevitable, and second, to suggest some alternate avenues of exploration toward the design of new kinds of languages.

This is pretty inflammatory stuff!

Backus talks about how the assignment statement is like the original sin of so-called Von Neumann style of programming: it’s like “what value is going into what register?” is the question we’re answering over and over again. This is not the same as an equals sign in mathematics!

The cure for all of this “cancerous growth” is functional programming!

…we give an informal description of a class of simple applicative programming systems called functional programming (FP) systems, in which "programs" are simply functions without variables.

The main reason FP systems are considerably simpler than either conventional languages or lambda-calculus-based languages is that they use only the most elementary fixed naming system (naming a function in a definition) with a simple fixed rule of substituting a function for its name.

To describe this system, he refers directly to McCarthy, and then gives various examples in the rest of the paper.

### ML

Just before Backus gives this Turing Award lecture, in 1973 ML was invented. ML an early 70s attempt to make a theorem-prover. It was meant to be a simply-typed lambda calculus, created by Robin Milner at the University of Edinburgh. You’ve probably heard of Robin Milner because of “Hindley-Milner type inference” and Milner later wrote this pretty seminal paper talking about how to produce a well-typed program in a polymorphic type system, which came out a year after Backus’ lecture around the time ML was formalized at the end of the 70s.

Landin’s work plus ML plus Scheme plus Backus’ lecture gave way to a whole bunch of work on functional programming in the 80s. Simon Peyton Jones and Philip Wadler have referred to both Backus and ML when talking about the origins of Haskell, which was created in the late 80s.

### Side note: Miranda(?)

Before Haskell, though we may mention Miranda. In 1985, there was this proprietary language called Miranda that was released, but you had to *pay to use it*. So the creators of Haskell got together and were like, “let’s write our own thing that we can use for our research and we’ll give it away for free.” (This was around 1989 and they have a funny photo of the dozen or so people involved in their first meeting about it, all wearing rad 80s sweaters).

### Wadler “Monads for Functional Programming”, 1995

So Haskell is dreamed up around 1989, a couple of years before Python is released. It’s intended to be a *lazy* language, based on simply-typed lambda calculus, using Church and lambda calculus but also the work of Robin Milner. It’s also a successor to Miranda: they wanted a “free” Miranda.

The thing is that a lazy language can evaluate expressions in any order. (Recall that expressions are like mathematical expressions; there’s no explicit “do this, then do that” encoding in these.) Because of this, they had to make the language pure: no side effects!

This means you can’t print to the screen. You can’t write a program which computes a value *and then displays that value to a user* because that *displaying the value is aside effect*! It’s not pure like a pure mathematical expression.

So this is a problem: these programs may work great, but who would ever know?

So among other attempts to fix this, they’re trying to figure out how to have side effects in a pure, lazy language and Wadler is inspired by this researcher Eugenio Moggi to use monads. He later writes a paper called “Monads for Funcational Programming” describing how these work to encode side effects and then another paper on how IO is implemente din Haskell using monads.

Anyway, we don’t have to turn this into a monads tutorial, but I’ve read this paper a bunch and I find it has a pretty distinctive and clear view on "Functional programming”.

Here’s how it starts:

The functional programming community divides into two camps. Pure languages, such as Miranda and Haskell, are lambda calculus pure and simple. Impure languages, such as Scheme and Standard ML, augment lambda calculus with a number of possible e↵ects, such as assignment, exceptions, or continuations. Pure languages are easier to reason about and may benefit from lazy evaluation, while impure languages offer efficiency benefits and sometimes make possible a more compact mode of expression.

…

Pure functional languages have this advantage: all flow of data is made explicit.

And this disadvantage: sometimes it is painfully explicit. A program in a pure functional language is written as a set of equations. Explicit data flow ensures that the value of an expression depends only on its free variables. Hence substitution of equals for equals is always valid, making such programs especially easy to reason about. Explicit data flow also ensures that the order of computation is irrelevant, making such programs susceptible to lazy evaluation.

He’s talking about *referential transparency*. I think you can sum up Wadler’s answer here as: “a functional programming language is based on lambda calculus.” I think out of that, he would laugh at those blog posts we used to see 10 years ago where people were talking about writing “functional-style” Javascript or Python. Further, you look at a framework like React and from the Wadler perspective, it might look kind of ridiculous to call it “functional programming.”

### General Discussion

Another viewpoint from Nathan:

*I'll try and think of a reference that tries to define what "functional" actually means. I'd maybe pose the slightly loose definition of "a language that requires nontrivial buy-in to break immutability and referential transparency"*

## Questions

- Did Rust stop FP momentum?
- Is Rust functional?
- Does popularity matter? (for example, Jane Street is deeply entrenched in OCaml)

### Does any of this matter anymore?

This stuff used to matter to me. It felt like we were right on the verge of making programs that were reliable, robust, mathematically-correct. We were using abstract algebra to assert higher-level truths about our programs.

Suddenly, it feels like this discussion doesn’t matter anymore though! In the face of AI-programming revolution: is anyone going to argue about this stuff in the future? Is it kind of like arguing about AC vs DC?

Will this type of discussion be like arguing about some low-level implementation that won’t matter to the vibe coders of the future?

(If it doesn’t they’re stuck with what we’ve got now! They’re going to be vibe coding python or rust for the foreseeable future…)

Side question: does the inability to vibe code a thing immediately doom new programming languages? They can’t achieve momentum or escape velocity without that: it means the most popular languages are likely going to increase their hegemony.

## References

[TIOBE Index](https://www.tiobe.com/tiobe-index/)

[The next 700 programming languages](https://www.cs.cmu.edu/~crary/819-f09/Landin66.pdf)

[What killed Haskell, could kill Rust, too · GitHub](https://gist.github.com/graninas/22ab535d2913311e47a742c70f1d2f2b)

[AN INTRODUCTION TO FUNCTIONAL PROGRAMMING THROUGH LAMBDA CALCULUS Greg Michaelson](https://www.macs.hw.ac.uk/~greg/books/gjm.lambook88.pdf)

Wikipedia on Lisp: [https://en.wikipedia.org/wiki/Lisp\_(programming\_language)](https://en.wikipedia.org/wiki/Lisp_%28programming_language%29)

McCarthy “Lisp Prehistory”: <https://www-formal.stanford.edu/jmc/history/lisp/node2.html>

ML: [https://en.wikipedia.org/wiki/ML\_(programming\_language)](https://en.wikipedia.org/wiki/ML_%28programming_language%29)

SPJ’s history fo Haskell: <https://simon.peytonjones.org/history-of-haskell/>

Wadler: <https://www.cs.tufts.edu/comp/150PLD/Papers/MonadsForFunctionalProgramming.pdf>

Backus Turing award lecture: <https://dl.acm.org/doi/epdf/10.1145/359576.359579>