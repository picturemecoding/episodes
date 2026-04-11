# Recreational Computing

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

Mike music: Dove Ellis - *Blizzard*

(we can both discuss this if you want)

When I first started programming I was mostly interested in figuring out how the machine worked. I liked to write assembly language and do silly things like write characters to the screen or draw lines in the video buffer or send characters through the parallel port to a dot matrix printer. It was fun, but it was hacking with no real intent beyond probing the capabilities of the computer. Even when I started to write things that could be called applications, the results were small chunks of code with primitive, array-like data structures, and control structures that just jumped to a label, or, when i got a bit more advanced, called subroutines.

Since I was not a computer science student, the first projects i worked on that had real algorithms and data structures were recreational computing projects. Many of these came from the *Computer Recreations* column written by A.K. Dewdney in Scientific American. *Computer Recreations* began as *Mathematical Recreations* and was a successor to Martin Gardner’s legendary *Mathematical Games* and Douglas Hofstadter’s *Metamagical Themas*.

The idea of recreational computing seems a bit quaint in 2026. Nobody’s likely to get rich doing recreational programs and in most cases AI tools can probably crank out decent code for the problem. It’s hard to understand, maybe, that people like me worked on these problems because they were *fun*, and maybe as a side-effect we learned something. But, mostly, it was for fun.

I think i wanted to do this because i feel nostalgic for this time. Is that bad? Is this kind of thing over?

Thoughts:

1. AI maybe made recreational programming irrelevant?
2. All of the leetcode bullshit ruined the idea of solving puzzles for fun?

### Core War

Dewdney was a particularly big influence on me because his column coincided with my early forays into computing. Dewdney was a professor of computer science in Canada, and somewhat weirdly, he became a 9/11 conspiracy theory promoter at some point.

Dewdney and Computer Recreations are probably best remembered for [Core War - Wikipedia](https://en.wikipedia.org/wiki/Core_War),

The basic idea behind Core War is that people code bots (though they weren’t called that) that compete against each other to control the “core” memory. As a game it was played in a virtual environment with a limited instruction set (called Redcode), because playing it in regular memory with the native instruction set would usually either hang the machine or core dump.

Usually two bots would start executing in different parts of the (virtual) memory of 8000 words, and then they’d write data or replicate themselves or whatever. The bots would take turns, each executing an instruction in a cycle. The game ends when the virtual machine tries to execute an invalid construction for one of the bots. This means that the opponent has written data over the memory location that holds the next instruction.

Core War didn’t teach me a lot about advanced programming, but it was interesting to think about how a computer could be simulated by a computer. This was in effect my first experience with a virtual machine.

### Flibs

The column that inspired me the most and stuck with me was about “finite living blobs” or “flibs”.

Flibs were a primitive sort of artificial life, basically a genetic algorithm. Each flib has “DNA” that is a finite state machine. The fitness of a given flib was determined by how well the DNA predicts an “environment”, which is basically a string of the symbols that the DNA/state machine transitions between. So, it’s sort of like regex pattern matching. Flibs that match well survive, and those that don’t die. New flibs are created through two mechanisms. First, there is a type of crossover “breeding” where parts of successful flibs are combined, and second, random mutations can change the state machine of a flib.

Programming flibs was fun for a few reasons. It was my first real experience with FA, and how to program them. Second, it required some slightly fancier data structures to facilitate the crossover breeding. Mostly though, it was fun to watch it evolve and unfold. Typically i’d see several generations of so-so scoring flibs, and then one would appear that matched the environment very well, and then it and its progeny would dominate going forward.

### Maze Solving

Did this with my college friends. It’s fairly easy to generate random mazes. Most solutions were variations of depth-first or breadth-first search on graphs, but we also tried stuff like “always turn right”, or just random guessing.

[E] I still have my copy of Mazes for Programmers and every so often I look at the code I wrote from that book.

### Fractals

I went through an extended period of messing with fractals, mostly Mandelbrot and Julia sets, but other stuff too.

### Game of Life

Maybe the best known computer recreation is John Horton Conway’s Game of Life, and cellular automata more broadly. I’ve never really “programmed” Life, just messed with existing simulations.

### Erik Coding for Fun

I made a taco-shop name generator in Haskell.

### Project Euler

[E] I did a bunch of these in every new language I wanted to learn.

### Breaking Stuff

[E] Do you remember how learning how something worked often involved changing pieces of it until it broke and then looking at what broke and why? Breaking stuff was kind of a method of fun-learning.

### Generalized Game Playing

I took a Coursera class in “generalized game playing”. The idea is that you create an agent that can play any two-player turn-based game. There are tournaments where you can upload your code and play against other agents. The game is described in an abstract way, so you don’t know if you’re playing tic tac toe or chess.

This does sort of have some practical applications, since it is (was?) an area of research in AI.

## Does Anybody Still Do This?

Apparently? There’s a biennial conference called Fun With Algorithms, which is in France this year.

Paper from FUN 2024, but the “MIT Hardness Group”:

[[2404.10380] PSPACE-Hard 2D Super Mario Games: Thirteen Doors](https://arxiv.org/abs/2404.10380)

[A Programming Language Embedded in Magic: The Gathering](https://drops.dagstuhl.de/storage/00lipics/lipics-vol291-fun2024/LIPIcs.FUN.2024.31/LIPIcs.FUN.2024.31.pdf)

These seem like fun, but they’re also pretty research-y

NOTE: I do not include things like LeetCode or HackerRank as recreational programming

## References

<https://cs.stanford.edu/~knuth/recreational-cs.pdf>

[FUN2026](https://fun2026.limos.fr/)

<https://playgameoflife.com/>

[Practicing the Fundamentals: The New Turing Omnibus](https://blog.codinghorror.com/practicing-the-fundamentals-the-new-turing-omnibus/)

[Project Euler](https://projecteuler.net/)