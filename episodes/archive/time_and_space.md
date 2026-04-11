# Time and Space

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

Mike Music: Nick Drake, *Five Leaves Left/The Making of …*

Erik Music: Aesop Rock *Black Hole Superette*. “maybe why I'm terrible with names is 'cause I'm special / At memorizing the exits during those same seconds”. “I’m too cold / even old man winter won’t dap the kid.”

## Main Topic

All programs take time and memory to run. Programmers know there’s often a tradeoff between using more memory (space) in order to make an algorithm run faster (time). For example, the mergesort algorithm will run in an amount of time proportional to n\*log(n), where n is the size of the list to be sorted, but it needs to use extra memory proportional to n.

Sometimes algorithms get analyzed in detail to try to determine worst-case and average-case performance. More often though, computer scientists talk about time and space usage with the language of computational complexity, which is more about determining boundaries and orders of magnitude.

## Complexity Classes: Time

Informally, programmers use big-O notation, eg, that algorithm is O(N). Algorithms where the running time is proportional to some power, O(n), O(n^2), etc are said to run in polynomial time. Computational complexity lingo groups all of these algorithms into a complexity class called P. (Technically P is the union of all classes DTIME(n^c), c >= 1).

Another class that people hear of is NP, which stands for non-deterministic polynomial. Most of these problems have exponential time complexity but by definition a computed answer can be *verified* in polynomial time.

### Simulations Use Turing Machines

Computer scientists use the theoretical construct of the Turing Machine to *simulate* and study different problems. A Turing Machine is a long strip of paper with cells on it, and you can imagine a thing that writes a value into each cell as it moves along, but it can also move backwards or to arbitrary places on the tape and *overwrite* or *erase* the value there. Eventually the Turing Machine stops or *halts* and the program is done.

It’s almost like a typewriter which can compute stuff, there’s a write head, a state, and a tape (I always imagine the state is somehow saved on the write head itself). “Programs” are sets of instructions like “if the state is 3 and the current cell reads 5, add three and move right four cells.”

In the computability theory stuff, Alan Turing came up with this idea and used it to demonstrate that certain types of problems can’t be *decided*: there’s no way to know for that problem if the Turing Machine would ever halt.

Now, there are different types of Turing Machines we can imagine:

- Single tape (one long, maybe infinite strip of paper)
- Multitape Turing Machine (various strips, any of which can be written by the write-head)

For the Complexity Classes involving time, they often ignore the single-tape/multi-tape thing, because interestingly you can turn a program for single-tape into one for multitape and vice-versa. It doesn’t dramatically affect the time in a way that the next distinction does.

For Complexity Classes involving time, they talk about two imaginary computing machines:

1. Deterministic Turing Machine: “Quick”, “easy” solutions (sometimes they can still take a while) that can be computed by one of these are in P.
2. Nondeterministic Turing Machine: “Quick”, “easy” solutions for a nondeterministic machine which can make a whole bunch of guesses at the same time. The problems that can be quickly computed on one of these are in NP.
   * Key idea: a solution to a problem that runs “quickly” on an NTM can be *verified* quickly on a deterministic Turing Machine
   * Various problems which can only be solved quickly on a NTM can be turned into each other *quickly* (on a deterministic turing machine)

A problem is said to be NP if it can be solved “easily” on a *nondeterministic* Turing Machine. Some problems in NP can alsobe solved “easily” on a *deterministic* Turing Machine, while others can’t. The problems that can be solved easily on a *deterministic* Turing Machine are in the complexity class P for polynomial time whereas problems that can be solved easily on a nondeterministic Turing Machine are called NP for *nondeterministic* polynomial. (People often misremember this as “non-polynomial”, but that’s not true! These are totally polynomial if you have a magical machine which can simultaneously simulate a bunch of guesses and pick one that works!).

One of the big open problems in computer science and math (a Millenium problem) is proving that NP and P are/are not equivalent.

- "There are a lot of things in this subject that we believe to be true that we don't know how to prove." (Michael Sipser from MIT)

It’s weird though, that the problems in NP-Complete

Definitions

- O() - We have two functions f, g. Then f = O(g) if there exists a constant c such that f(n) <= c \* g(n) for every sufficiently large n
- o() - little o, f = o(g) if for every epsilon > 0, f(n) <= eps \* g(n) for every sufficiently large n
- Ω() - f = Ω(g) if g = O(f)
- ϴ() - f = ϴ(g) if f = O(g) and g = O(f)

### Example Problems

- P problems: <https://en.wikipedia.org/wiki/P-complete>
- NP-Complete problems: <https://en.wikipedia.org/wiki/List_of_NP-complete_problems>

Problems in P are often familiar, everyday problems we find at work: sorting lists, finding a path in a graph, etc.

Problems in NP which are not known to be in P are hard problems:

- Hamiltonian path problem is in NP. Non-determinism is important for NP because there's a parallelism that allows checking all the paths on different branches. Use non-determinism to try all possible paths. If you could spontaneously generate a whole bunch of guesses, you could check these in polynomial time using a *deterministic* turning machine.
- Traveling Salesman
- SAT

How weird is it that these problems which don’t seem to have anything to do with each other? I used to like doing sudoku puzzles, for example, but generalized sudoku, where you have an arbitrarily large grid is like a reflection of these other problems, Hamiltonian path, 3SAT, etc.

### Question: Have you ever used this theoretical knowledge at work?

- Any situation which required knowing a problem was NP Complete?

### Question: How does the Hierarchy Theorem fit in? (skip, probably?)

- reference: https://en.wikipedia.org/wiki/Time\_hierarchy\_theorem

## Complexity Classes: Space

It’s less common for programmers to talk about space classes, although the ideas are similar. For example, algorithms that require an amount of memory that is related to some polynomial of the input size are in PSPACE. Somewhat confusingly the space classes encompass time classes that have higher complexity, for example PSPACE includes *all* NP problems.

From the [Wired](https://www.wired.com/story/for-algorithms-a-little-memory-outweighs-a-lot-of-time/) piece:

Every problem in P is also in PSPACE, because fast algorithms just don’t have enough time to fill up much space in a computer’s memory. If the reverse statement were also true, the two classes would be equivalent: Space and time would have comparable computational power. But complexity theorists suspect that PSPACE is a much larger class, containing many problems that aren’t in P. In other words, they believe that space is a far more powerful computational resource than time. This belief stems from the fact that algorithms can use the same small chunk of memory over and over, while time isn’t as forgiving—once it passes, you can’t get it back.

“The intuition is just so simple,” Williams said. “You can reuse space, but you can’t reuse time.”

## Ryan Williams Result

Space vs time: tradeoffs, how to think about it?

![](data:image/png;base64...)

I love that line “We find Theorem 1.1 to be very surprising…”

Tree evaluations as a foundation…

- Stephen Cook et al proof about tree evaluations: memory can’t be compressed (nice demo on this in the chalk talk video about swapping two variables in memory: you need to use a temporary variable, right??)

What can we say here?

- It is very surprising
- Uses ideas from the Hopcroft, Paul, Valiant paper
- Cook (who originated the NP idea but didn’t name it that) did a paper that used pebbling to determine the lower bound on memory needed for certain tree evaluations. He offered a $100 bounty to anyone who could improve on it.
- Cook’s son et al wrote a paper on how to do the problem with less memory using a procedure where memory locations are sort of shared (the XOR/roots of unity thing).
- Williams put this all together to show that any t(n) problem can be done in sqrt(t(n)/log(t(n)) space.
- Computer scientists believe that P != PSPACE but nobody has proved it. This provides more evidence but doesn’t solve it. If you show that P != PSPACE it also proves P != NP.

### Previous Results

- Single tape Turing Machine: space vs time. You generally need like logn space vs n time on a single-tape machine?
- Multitape turing machine results were larger?

The pebbles thing…

The squishy pebbles thing…

- “You can store a variable while also modifying it” (from the chalk talk video). This is kind of nuts to think about. XOR example…
- Roots of unity: use the same memory over and over to store and compute values
- There seems to be some nutty math in here where you can store a computed value and an input *together*…

Stephen Cook’s proof had assumed that bits of data were like pebbles that have to occupy separate places in an algorithm’s memory.

But his son instead came up with a different take: “We can actually think about these pebbles as things that we can squish a little bit on top of each other,” Beame said.

Here’s an idea: vibe-code a database that uses this space-saving stuff to store everything on a tiny tiny amount of space, like on a business card? “Databases on business cards” is my pitch: it sounds like an Aesop Rock lyric…

From the [paper](https://dl.acm.org/doi/10.1145/322003.322015):

It is shown that every deterministic multitape Turing machine of time complexity t(n) can be simulated by a deterministic Turing machine of tape complexity t(n)/logt(n)...

Our simulation reduces the problem of simulating time-bounded multitape Turing machines to a series of implicitly-defined Tree Evaluation instances with nice parameters, leveraging the remarkable space-efficient algorithm for Tree Evaluation recently found by Cook and Mertz

From the [Wired](https://www.wired.com/story/for-algorithms-a-little-memory-outweighs-a-lot-of-time/) piece:

“I just thought I was losing my mind,” said Williams, a theoretical computer scientist at the Massachusetts Institute of Technology.

## References

- [For Algorithms, Memory Is a Far More Powerful Resource Than Time | WIRED](https://www.wired.com/story/for-algorithms-a-little-memory-outweighs-a-lot-of-time/)
- [Astonishing discovery by computer scientist: how to squeeze space into time](https://www.youtube.com/watch?v=8JuWdXrCmWg)
- <https://arxiv.org/pdf/2502.17779>
- [On Time Versus Space | Journal of the ACM](https://dl.acm.org/doi/10.1145/322003.322015)
- P problems: <https://en.wikipedia.org/wiki/P-complete>
- NP-Complete problems: <https://en.wikipedia.org/wiki/List_of_NP-complete_problems>

Erik notes

- We should talk about "the hierarchy theorem": how much bigger do you have to take the bound before you get something new
- Odd comment: “when you have an answer from a non-deterministic turing machine, you can’t simply *invert* that answer. Non-determinism doesn’t work that way.” (In response to the question of whether complement to a Hamiltonian path algorithm is *also* in NP, where the reasonable answer is “nobody knows”)