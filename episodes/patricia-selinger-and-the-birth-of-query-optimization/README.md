# Patricia Selinger and the Birth of Query Optimization

## Episode Overview

A deep dive into one of the most influential papers in database history: Patricia Selinger's 1979 SIGMOD paper "Access Path Selection in a Relational Database Management System." We explore how Selinger and the IBM System R team invented cost-based query optimization — a technique so sound that it underpins virtually every relational database in use today.

**Episode type:** Topic\
**Hosts:** Mike & Erik ---

## Music Segment

Erik Music:

Mike Music:

------------------------------------------------------------------------

## Background & Historical Context

### The Problem Before System R

We talked about the System R project in our episodes on Jim Gray and relational databases. Many of the ideas we associate with relational databases came out of System R, including ACID transactions, locking, consistency constraints. However, it wasn't clear that relational databases could be made efficient. Query optimization was a major step toward accomplishing that, and to nobody's surprise that also came out of the System R group.

The main driver beyond the query optimizer in System R was Particia (Pat) Selinger, an applied mathematician who joined IBM from Harvard. A lot the ideas outlined by Selinger are still in use today.

### Patricia Selinger Joins IBM (1975)

Patricia Griffiths Selinger received her A.B., S.M., and Ph.D. in **Applied Mathematics** from Harvard University (1971–1975) — notably *not* computer science. She joined IBM Research in San Jose the same year she finished her PhD and quickly became a central figure on the System R team.

Her background in applied math, with its emphasis on optimization and cost modeling, turned out to be exactly the right lens for the problem she'd tackle: how do you automatically choose the best execution plan for a query?

### The Challenge of Query Optimization

When a user writes:

``` sql
SELECT name FROM employees, departments
WHERE employees.dept_id = departments.id
AND departments.location = 'New York'
```

There are many ways to execute this: - Which table do you scan first? - Do you use an index or a full scan? - For a join of N tables, there are N! possible orderings — for 10 tables that's 3.6 million

Before Selinger's work, systems used **heuristics** — rule-of-thumb decisions baked in by programmers. The System R team wanted something better: a principled, *cost-based* approach.

I think when most programmers start working with SQL they're unaware that there are potentially different ways in which the query can be executed. What probably happens is that a query turns out to be slower than expected so that the programmer is motivated to figure out why. As some point in this process she discovers EXPLAIN, a keyword that can be prepended to a query to show the query plan. The plan is the strategy that the database engine uses to get the requested records in the most efficient way. The efficiency is treated in terms of *cost*, which is basically a measure of the I/O and computation needed to execute the query.

Some things that effect the plan:

-   Indexes

-   Predicates (sargable vs non-sargable)

-   Joins

-   Ordering (and grouping)

### The Single Table Case

Two possibilities: - Segment scan (scan the whole table) - Index scan

Basic cost function: COST = PAGE FETCHES + W \* (RSI CALLS)

The optimizer has to get a set of statistics from the database catalog

> The number \> of expected RSI calls (RSICARD) is the product of the relation cardinalities times the \> selectivity factors of the sargable boolean factors,

This is an important bit, because the cost-based optimizer is dependent on getting accurate cardinalities for relations and indexes.

> To find the cheapest access plan for a single relation query, we need only to examine the cheapest access path which produces tuples in each “interesting” order and the cheapest “unordered” access path.

#### Examples

``` SQL
SELECT a,b FROM C
```

-   Segment scan is only option because there are no predicates

```         
SELECT a,b FROM C WHERE a = 10
```

-   Paths
    -   Full scan
    -   Index scan onfull scan or index scan if there's an index on A
    -   Probably uses index scan path (i think maybe there's case where the index loses, say if there are only 2 values of "a")

```         
SELECT a,b FROM C WHERE a = 10 and b > 100
```

-   Paths
    -   full scan
    -   index scan on a
    -   index scan on b

```         
SELECT a, b FROM C WHERE a = 10 ORDER BY b
```

This is the case where *interesting orders* comes into play. The paths here are - Full scan, then sort by b - Index scan on a then sort by b - Index scan on b (will already be sorted)

In this case even if the "a" index is lower cost than the "b" index, the cost has to included the extra sort step.

### Joins

Joins get hard because you have to consider all of the stuff from the single relation case, but also the *order* in which joins occur, and also which type of join will be best.

Even if the query has the tables in a particular order, the planner might use a different order that's more efficient. The *result* is the same. If you have N tables in the FROM, there are N! factorial possible join orders, which is too many when N \> 3 or so.

> A heuristic is used to reduce the join order permutations which are considered. When possible, the search is reduced by consideration only of join orders which have join predicates relating the inner relation to the other relations already participating in the join

Two types of join considered: nested loop, and merge join. Merge join takes advantange of indexes on join columns, which avoids needing to rescan entire inner relation.

> The search tree is constructed by iteration on the number of relations joined so far. First, the best way is found to access each single relation for each interesting tuple ordering and for the unordered case.
> Next, the best way of joining any relation to these is found, subject to the heuristics
> for join order. This produces solutions for joining pairs of relations. Then the best
> way to join sets of three relations is found by consideration of all sets of two relations and joining in each third relation
> permitted by the join order heuristic

I think this is the "dynamic programming" approach? I think the cost in joins is always based on the scan of "outer" join plus the scan of the "inner", but the outer join might be the cumulative results of prior joins.

So the basic approach is that they build a tree of possible approaches for joining the tables, and then choose the "path" with the lowest cost. Most of the cost is again just cardinalities of the relations, but again sort orders and join methods have to be considered.

> The number of solutions which must be
> stored is at most 2\*\*n (the number of subsets of n tables) times the number of interesting result orders

Example from paper:

```         
SELECT NAME,TITLE,SAL,DNAME
FROM EMP,DEPT,JOB
WHERE TITLE=‘CLERK’
AND LOC=‘DENVER’
AND EMP.DNO=DEPT.DNO
AND EMP.JOB=JOB.JOB
```

In the join, they first look at scanning options for EMP, DEPT, and JOB.

Then they work on the join (EMP, DEPT), (EMP, JOB), (DEPT, EMP) and (JOB, EMP), but not (DEPT, JOB) or (JOB, DEPT) because there's not join predicate for them.

------------------------------------------------------------------------

## The Seminal Paper

**"Access Path Selection in a Relational Database Management System"**\
Patricia G. Selinger, Morton M. Astrahan, Donald D. Chamberlin, Raymond A. Lorie, Thomas G. Price\
*Proceedings of ACM SIGMOD, 1979*

-   📄 [PDF (Duke CS)](https://courses.cs.duke.edu/compsci516/cps216/spring03/papers/selinger-etal-1979.pdf)
-   🔗 [Semantic Scholar](https://www.semanticscholar.org/paper/Access-path-selection-in-a-relational-database-Selinger-Astrahan/7def002796277facffe02aa09e3a1bb101ec0785)
-   🔗 [IBM Research entry](https://research.ibm.com/publications/access-path-selection-in-a-relational-database-management-system)

### Core Ideas

#### 1. Cost-Based Optimization

Instead of hardcoded heuristics, the optimizer enumerates candidate execution plans and assigns each an *estimated cost* — measured in I/O operations and CPU time. It then selects the cheapest plan. Cost estimates are derived from **catalog statistics** (cardinality of tables, number of distinct values, etc.).

#### 2. Selectivity Estimation

For a predicate like `age > 30`, the optimizer estimates what fraction of rows will match — the **selectivity factor**. The paper introduced formulas still recognizable in modern systems: - Equality predicate on a column with K distinct values → selectivity = 1/K - Range predicate → selectivity = (max - value) / (max - min) - Multiple predicates joined by AND → multiply selectivities (independence assumption)

This independence assumption is famously a source of error and has driven decades of follow-on research (histograms, sketches, ML-based cardinality estimation).

#### 3. Dynamic Programming for Join Ordering

Rather than exhaustive search (N! plans), the optimizer uses **dynamic programming**: build optimal plans for subsets of relations bottom-up, exploiting the principle of optimality. This reduces the search space from factorial to exponential — still expensive for very large joins, but tractable for the typical case.

System R focused specifically on **left-deep plans** (linear join trees where one input is always a base relation), which further constrains the search space and maps naturally to a pipeline execution model.

#### 4. Interesting Orders (the Subtle Genius)

This is perhaps the paper's most elegant insight. When evaluating plans, you don't just track the cheapest plan for each subset of relations — you also track the cheapest plan *for each "interesting ordering"* of the output.

An ordering is **interesting** if it's useful to some later operator: e.g., if the query has `ORDER BY dept`, or if a downstream join can exploit the sort. A plan that's slightly more expensive but produces sorted output may end up being globally optimal because it avoids a sort step later.

This idea — tracking a Pareto frontier of cost vs. output properties — is a precursor to modern concepts like physical properties in the Volcano/Cascades optimizer frameworks.

------------------------------------------------------------------------

## Key Concepts for Discussion

-   **Declarative vs. procedural queries**: why giving up control to the optimizer felt radical in 1979
-   **The cost model**: what statistics System R maintained and how they estimated I/O vs. CPU costs
-   **The independence assumption**: simple, wrong, and still everywhere — what's been done about it?
-   **Left-deep vs. bushy join trees**: why System R restricted the search space and when that matters
-   **Interesting orderings**: how this small insight has giant downstream effects
-   **The optimizer as a compiler**: query optimization as a code generation problem
-   **How the System R optimizer became DB2**: the paper's ideas were taken nearly verbatim into production

------------------------------------------------------------------------

## Timeline

| Year | Event |
|---------------------------------|---------------------------------------|
| 1970 | Codd publishes "A Relational Model of Data for Large Shared Data Banks" |
| 1973 | IBM System R project begins at San Jose Research Lab |
| 1974 | Chamberlin & Boyce publish SEQUEL (precursor to SQL) |
| 1975 | Selinger joins IBM Research after Harvard PhD |
| 1979 | "Access Path Selection" paper published at SIGMOD |
| 1981 | System R prototype officially described in a landmark TODS paper |
| 1983 | IBM DB2 ships, optimizer derived directly from Selinger's work |
| 1987 | Graefe & DeWitt publish the Volcano optimizer framework (builds on Selinger's ideas) |
| 1994 | Selinger named IBM Fellow |
| 1995 | Graefe publishes the Cascades framework — modern successor to Volcano |
| 1999 | Selinger elected to National Academy of Engineering |
| 2002 | SIGMOD Edgar F. Codd Innovations Award |
| 2018 | Selinger retires from IBM |

------------------------------------------------------------------------

## Discussion Questions

1.  **On the leap of faith**: In 1979, trusting a machine to pick the best query plan felt radical. What had to be true about the state of computing for this to be accepted? Would it have been accepted earlier? Later?

2.  **The math background angle**: Selinger came from applied mathematics, not CS. How much did that shape the cost-based framing vs. a more CS-heuristics approach?

3.  **The independence assumption**: The paper assumes column predicates are independent when estimating selectivity. Every database researcher knows this is wrong. Why has it persisted for 45 years?

4.  **Interesting orders as a design principle**: The idea of tracking "useful properties" of intermediate results beyond just cost is powerful. Where else in computing do we see this kind of multi-objective optimization?

5.  **From research to product**: The System R optimizer went into DB2 "lock, stock, and barrel." How often does that happen? What made System R unusual?

6.  **Modern query optimization**: With ML-based cardinality estimation (Bao, Neo, etc.) getting real traction in the 2020s, is the Selinger approach finally being superseded? Or augmented?

7.  **The join ordering problem is NP-hard**: Modern systems cheat in various ways (heuristics above a join threshold, genetic algorithms, memoization). What are the tradeoffs?

------------------------------------------------------------------------

## Interesting Stories & Angles

-   **The skeptics were loud**: Mike Stonebraker was a prominent critic who argued relational databases would always be too slow. System R was a direct rebuttal. Selinger's optimizer was a key reason the performance story improved.

-   **The paper almost didn't happen the way it did**: Selinger has described in interviews how the team debated whether to publish — IBM was protective of the work given its commercial value. The decision to publish at SIGMOD 1979 shaped the entire field.

-   **The "interesting orders" naming**: The term itself is charmingly informal for something so mathematically precise. It's remained in the literature for 45+ years.

-   **The ACM Queue interview (2006)**: Conducted by James Hamilton (later of AWS fame), Selinger is candid about the evolution of the optimizer, what she'd do differently, and the challenge of moving research to product. Worth quoting directly.

-   **The CACM Database Dialogue (2008)**: Another rich interview where she reflects on 30+ years of database history and the state of the field.

------------------------------------------------------------------------

## Further Reading

-   [Access Path Selection paper (PDF)](https://courses.cs.duke.edu/compsci516/cps216/spring03/papers/selinger-etal-1979.pdf)
-   [A Conversation with Pat Selinger — ACM Queue (2006)](https://queue.acm.org/detail.cfm?id=1059803)
-   [Database Dialogue with Pat Selinger — CACM (2008)](https://cacm.acm.org/magazines/2008/12/3355-database-dialogue-with-pat-selinger/fulltext)
-   [Pat Selinger Speaks Out — SIGMOD Interview (PDF)](https://sigmod.org/publications/interviews/pdf/17.selinger-interview.pdf)
-   [Patricia Selinger — IBM History](https://www.ibm.com/history/patricia-selinger)
-   [System R: Database Research Retrospective — TODS 1981](https://dl.acm.org/doi/10.1145/319996.319997)
-   Graefe, G. (1995). *The Cascades Framework for Query Optimization* — direct intellectual successor
-   Leis et al. (2015). [*How Good Are Query Optimizers, Really?*](https://vldb.org/pvldb/vol9/p204-leis.pdf) PVLDB Vol. 9 — introduces the Join Order Benchmark (JOB) and empirically audits modern optimizers; finds cardinality estimation errors (not cost models or plan enumeration) are the dominant cause of bad plans — a direct stress-test of Selinger's independence assumption

------------------------------------------------------------------------

*Research compiled: April 2026*