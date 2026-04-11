# Standing on the Shoulders of Giants: Frances Allen and Compilers

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

Erik Music: Telekinetic Yeti and their album *Primordial*. They’re from Dubuque Iowa this is stoner doom. They have a song called “Ancient Nug” and another song called “Stoned Ape Theory” and another song called “Toke Wizard”, which is actually a super rad song. So this is music to fade out of society too. I tend to talk about a lot of stoner music, it seems? It’s funny for someone who (no judgments) doesn’t really partake of it. Someone who doesn’t know me might assume some things… I just have an appreciation for the culture, I guess? Well, if you want to get your feet wet in the genre and you don’t want to listen to one song that’s an hour long (because it feels like too big of a commitment or something), check out this record. Start with “Toke Wizard” and let it wipe away your concerns.

Mike Music: The Messthetics and James Brandon Lewis

The Messthetics 3 guys, two of whom are the former rhythm section of Fugazi.

Mike it’s now December and December for me is *compiler season*.

## Compilers 101

- “Frontend”: Lexer, Parser, AST, Semantic Analysis
- “Middle end”: Optimizations
- “Back end”: Machine code, register allocation

## Biography

- Born August 4th in 1932, died of alzheimer’s disease in 2020 on her 88th birthday! She was actually [profiled in the New York Times](https://www.nytimes.com/2020/08/08/technology/frances-allen-dead.html?partner=IFTTT) and other newspapers ([the verge](https://www.theverge.com/2020/8/9/21360722/frances-allen-computer-scientist-compiling-ibm), cnn, [IEEE](https://www.computer.org/csdl/magazine/co/2020/11/09237322/1o8m1HT6zT2)) when she died.
- An [interview with her](https://dl.acm.org/doi/10.1145/1866739.1866752) on ACM ([PDF version](https://dl.acm.org/doi/pdf/10.1145/1866739.1866752))
- Worked at IBM
- “When I joined IBM in 1957, I had a master’s degree in mathematics from the University of Michigan, where I had gone to get a teaching certificate to teach high school math. But I had worked on an IBM 650 there, so I was hired by IBM Research as a programmer. My first assignment was to teach FORTRAN, which had come out in the spring of that year.”
- From the ACM:

To teach herself how the FORTRAN compiler worked, she read its source code. Thus began her interest in compilers. Her "throwaway job," as she called it at first, proved so compelling that she stayed 45 years, becoming the first female IBM Fellow in 1989. [[cacm](https://cacm.acm.org/news/fran-allen/)]

- Increased the representation of women at IBM:
  + In a field long dominated by men, Ms. Allen was a force for change. In the 1970s and ’80s, thanks largely to her own efforts, women accounted for half of the experimental compiler group inside IBM. “One the many things Fran did was attract women to her field,” said Jeanne Ferrante, who worked alongside Ms. Allen for more than a decade. “She looked out for the people who were underrepresented.”
- (footnote: John Backus also worked at IBM overlapping in time)
- Later career at IBM: parallel computing, to automatically parallelize computations.

## Career Highlights

From the [wikipedia entry](https://en.wikipedia.org/wiki/Frances_Allen#Career_and_research) on her career, her A. M. Turing Award citation reads:

Fran Allen's work has had an enormous impact on compiler research and practice. Both alone and in joint work with John Cocke, she introduced many of the abstractions, algorithms, and implementations that laid the groundwork for automatic program optimization technology. Allen's 1966 paper, "Program Optimization," laid the conceptual basis for systematic analysis and transformation of computer programs. This paper introduced the use of graph-theoretic structures to encode program content in order to automatically and efficiently derive relationships and identify opportunities for optimization. Her 1970 papers, "Control Flow Analysis" and "A Basis for Program Optimization" established "intervals" as the context for efficient and effective data flow analysis and optimization. Her 1971 paper with Cocke, "A Catalog of Optimizing Transformations," provided the first description and systematization of optimizing transformations. Her 1973 and 1974 papers on interprocedural data flow analysis extended the analysis to whole programs. Her 1976 paper with Cocke describes one of the two main analysis strategies used in optimizing compilers today. Allen developed and implemented her methods as part of compilers for the IBM STRETCH-HARVEST and the experimental Advanced Computing System. This work established the feasibility and structure of modern machine- and language-independent optimizers. She went on to establish and lead the PTRAN project on the automatic parallel execution of FORTRAN programs. Her PTRAN team developed new parallelism detection schemes and created the concept of the program dependence graph, the primary structuring method used by most parallelizing compilers.

— Association for Computing Machinery (ACM), Citation for the A. M. Turing Award 2006

## Papers

- 1970: “[Control Flow Analysis](https://se421-fall2018.github.io/resources/readings/p1-allen.pdf)”
- 1971: “[A Catalog of Optimizing Transformations](https://www.clear.rice.edu/comp512/Lectures/Papers/1971-allen-catalog.pdf)”
- 1976: “[A Program Data Flow Analysis Procedure](https://dl.acm.org/doi/10.1145/360018.360025)”

## Compiler Contributions Enumerated

1. Graph data structures (“Program Optimization”)
2. Systematic analysis (“Control Flow Analysis”)
3. Various optimizations on top of this

### Program Optimization (1966 IBM internal, 1969 a journal)

I cannot find a copy of this paper which introduced using graphs to optimize programs in compilation:

“Her first, "Program Optimization," was distributed internally at IBM in 1966 and published in the 1969 Annual Review in Automatic Programming.” [[cacm](https://cacm.acm.org/news/fran-allen/)]

“The use of a nested-set of strongly connected regions in control flow analysis for optimization was first suggested in [this paper]. In that approach to control flow analysis, a set, D, of disjoint sets of nested strongly connected regions is found.” (from the 1970 paper “Control Flow Analysis”)

The above is useful but these sets are partially ordered and it would be useful to have a whole program ordering. See paper below for more information!

### Control Flow Analysis (1970)

This paper talks about: 1) graph data structures generally and with respect to compilers, 2) “dominance relationships” (an idea that had appeared previously), 3) “Intervals”, which is the real contribution of this paper, 4) “partitioning graphs by intervals”, and 5) an example program.

Commentary on the above: this graph structure and resulting analysis allows a compiler to consider questions like:

- Where is this variable referenced?
- Is this an inner loop?
- Can we move a variable declaration *out* of the loop?

#### 1. Basic graph rules for compilers

- Directed, connected graphs (where any node can be reached by any other node by successive applications of successor->node and predecessor->node.
- ‘A basic block [the paper uses “block” for “node”] is a linear sequence of program instructions having one entry point (the first instruction executed) and one exit point (the last instruction executed).’
- Program entry blocks may not have predecessors in the program. Program terminating blocks never have successors in the program.
- A “control flow graph” is a digraph in which nodes represent basic blocks and edges are control flow paths.
- A circuit is a loop, where you start and end on the same node. If all nodes in the circuit are distinct (aside from beginning-ending) then it’s a simple circuit, otherwise it’s a composite circuit.

#### 2. “Dominance Relations”

Assume we introduce a single arbitrary entry node (a real entry node may be returned to in a closed circuit, so we just imagine an arbitrary node as the predecessor to any entry point nodes) and then we have one or more exit nodes.

A “back dominator” node is one which you have to go through on any paths in the graph to get to a successor node. Use this graph from the paper: 2 is the immediate back dominator of 5:

![](data:image/png;base64...)

The interesting thing is that these “back dominators” are strictly ordered by the distance function. A forward dominator is the inverse: “every path *away* from X must go through Y, so it’s a forward dominator.” In the above example: 5 is a forward dominator of 2 and 2 is a back dominator of 5. Ultimately, a node that is on *every* entry-exit path is an “articulation node”: [1, 2, 6] above. With this, we can now talk about *intervals*.

#### 3. “Intervals”

This is a way of partitioning up the graph into *unique* intervals. They give a linear pass algorithm to traverse all edges once and build up a set of intervals. I struggled with this so I had claude generate some code which [I put into a gist](https://gist.github.com/erewok/cb0ac3822207aebb83f7ec36e44c11e4).

The short version is that you look for each node which has only a single predecessor.

When processing 'D', it has predecessors from multiple intervals, so it becomes a new header.

![](data:image/png;base64...)

We can also talk about entry-exit nodes for the intervals and an articulation path for the interval. Careful though: each header node is not automatically an articulation node for the whole graph.

Procedure:

1. Find all back-dominators in an interval for interval exits
2. Use the above to find the interval articulation nodes

#### 4. Partitioning a Graph into Intervals

The paper then talks about how to partition graphs and ultimately how to determine if they are reducible (where each excursion into an interval can be reduced down to just the entry node, which back-dominates all intervals following.

Question: She mentions in the ACM interview that Tarjan’s Spanning Trees obviated the need for creating these intervals (and his work was more efficient). It seems like if you can cut the graph using minimum spanning trees and cut points, you can reach the same conclusions?

#### 5. Why It Matters

You can *reduce* each interval to its header. Why would you do this? With all of the above we can now talk about the benefits to analyzing a whole program: “...information of *global interest* is left at the interval head and at the exits.”

This seems really due to human cognition limits (structured programming anyone…?) but they found that 90% of the FORTRAN programs they analyzed were *reducible*.

The fascinating thing to me about this paper is that we do this all the time when reasoning about and refactoring programs. We try to figure out “where is *this information made available*”, in other words, “where is this value used?” If I can isolate some subroutine to a whole bunch of local variables, I know, for example, the renaming those variables within that subroutine will have no effect on the whole program. Allen’s paper lays out the graph operations that allow a compiler to understand these relationships.

### A Catalog of Optimizing Transformations (1976)

“A result of the recent work in optimization has been to systematize the potpourri of optimizing transformations that a compiler can make to a program. This paper catalogs many of these transformations.”

First point: it’s not obvious how to programmatically determine “optimal” code, so this paper is more about “ameliorations”: can we help fix pathological code with lots of unnecessary instructions? Another problem is that it’s not always obvious if we have preserved correctness. Thus, the goal is to preserve side effects and do whatever the code did by just eliminating common mistakes.

Structure of the paper

1. “Interprocedural optimizations are presented first”
2. “Transformations best performed on a program form close to the source language are presented next,” followed by
3. The “so-called” machine-independent optimizations and
4. Finally machine dependent optimizations

Example: eliminating redundant instructions is machine independent whereas register allocation is machine dependent.

#### Interprocedural

Different ways to collapse C -> S (caller and callee) described by their relationship

- Closed (no optimization performed): no information about S is available when compiling C. It’s inefficient to link (no benefits: no shared arguments, variables, registers, etc.).
- Open (completely collapsed together), but problems: it may get big, or lots of side-effects make this pretty freaking tough
- Semi-Open (some optimization: overlapping values, one shared module)
- Semi-Closed (where *knowledge* of S can aid *compilation* of C). Problem: recompiling S means recompiling all callers!

Semi-open and semi-closed are “non-standard”.

#### Loop Transformations

1. Loop Unrolling
2. Jamming or Loop Fusion (when you have the same loop used twice, you can sometimes jam the guts of each loop together)
3. Unswitching (the opposite of jamming)

#### Redundant Subexpression Elimination

Find and eliminate computations which calculate values already available.

#### Code Motion

(This is a good name for our future reggae band, Mike). Moves code around for fewer instructions. Ex: assigning a constant value inside a loop.

#### Constant Folding

Just stuff the constant into the hole where it’s referenced instead of having to lookup a variable value.

#### Dead Code Elimination

“Primarily as a result of constant folding, instructions become dead. Instructions are considered dead when they cannot be executed because they are in an area of the program which cannot be reached, or when their results are never used.”

#### Strength Reduction

“The strength reduction optimization replaces certain computations using recursively defined

variables by recursively defined computations.” Variables defined recursively -> in terms of each other. Build out earlier computations instead and refer to those.

#### Linear Function Test Replacement

Sometimes as a result of other optimizations, there are functions that are now just testing the results of functions.(?)

#### Carry’s

“The carry optimization is a somewhat specialized optimization which recognizes when a subscript calculation for referencing sequential elements of an array does not need to be re-initialized when the reference changes to the next row or column.”

[Note: Remaining optimizations are *machine dependent*, prefixed by “MD”]

#### [MD] Instruction Scheduling

Move instructions to take advantage of assigning values to registers for fewer instructions.

#### [MD] Parsing Methods

…

#### [MD] Register Allocation

“...quite possibly the hardest optimization to perform.”

#### [MD] Storage Mapping

Space and adjacency: decrease dead space and colocate stuff used together.

#### [MD] Shadow Variables

Example: some decimal numbers are easier to deal with as binary (due to operations). Keep the originals but also make a copy in binary to do the easier computations with these.

#### [MD] Anchor Pointing

For branches, sounds like early returns

#### [MD] Special Case Code Generation

Ex: A^2 -> A\*A without using exponentiation

#### [MD] Peephole or Window Optimization

Look over an interval of instructions and optimize these