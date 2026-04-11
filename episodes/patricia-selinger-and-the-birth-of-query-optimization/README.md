# Patricia Selinger and the Birth of Query Optimization

## Episode Overview

A deep dive into one of the most influential papers in database history: Patricia Selinger's 1979 SIGMOD paper "Access Path Selection in a Relational Database Management System." We explore how Selinger and the IBM System R team invented cost-based query optimization, a technique that still underpins virtually every relational database in use today.

**Episode type:** Topic\
**Hosts:** Mike & Erik

## Music Segment

Erik Music:

Mike Music: *Into Oblivion* - Lamb of God

A couple of metal bands that have been around for a long time recently put out new albums. One is *An Undying Love for a Burning World* by Neurosis, which garnered a lot of critical acclaim (currently a 92 on Metacritic). The other is *Into Oblivion* by Lamb of God. I like them both but sort of prefer the latter. It's super aggro, and a little bit cliche at points, but this is kind of what i needed right now. My favorite track is The Killing Floor, which has the charming chorus:

> Bow down to the butcher,
>
> Slaughtering your future,
>
> On the killing floor.

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

#### Cost-Based Optimization

Before Selinger's work, systems used **heuristics** — rule-of-thumb decisions baked in by programmers. The System R team wanted something better: a principled, *cost-based* approach. Here "cost" does not literally mean \$, but rather the amount of I/O and computation.

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

**How does SQL processing work internally?**

> Each SQL statement is sent to the parser, where it is checked for correct syntax. A query block is represented by a SELECT list, a FROM list, and a WHERE tree, containing, respectively the list of items to be retrieved, the table(s) referenced, and the boolean combination of simple predicates specified by the user.

> The four phases of statement processing are **parsing**, **optimization**, **code generation**, and **execution**.

This paper is mostly going to talk about **optimization** and **code generation**

> After a plan is chosen for each query block and represented in the parse tree, the CODE GENERATOR is called. The CODE GENERATOR is a table-driven program which translates ASL trees into machine language code to execute the plan chosen by the OPTIMIZER. In doing this it uses a relatively small number of code templates, one for each type of join method (including no join).

I was surprised there's a lookup table driving this?

#### Selectivity Estimation

For a predicate like `age > 30`, the optimizer estimates what fraction of rows will match — the **selectivity factor**. The paper introduced formulas still recognizable in modern systems:

-   Equality predicate on a column with K distinct values → selectivity = 1/K

-   Range predicate → selectivity = (max - value) / (max - min)

-   Multiple predicates joined by AND → multiply selectivities (independence assumption)

This independence assumption is famously a source of error and has driven decades of follow-on research (histograms, sketches, ML-based cardinality estimation).

#### Examples

``` sql
SELECT a,b FROM C
```

-   Segment scan is only option because there are no predicates

``` sql
SELECT a,b FROM C WHERE a = 10
```

-   Paths
    -   Full scan
    -   Index scan onfull scan or index scan if there's an index on A
    -   Probably uses index scan path (i think maybe there's case where the index loses, say if there are only 2 values of "a")

``` sql
SELECT a,b FROM C WHERE a = 10 and b > 100
```

-   Paths
    -   full scan
    -   index scan on a
    -   index scan on b

##### Interesting Orders

``` sql
SELECT a, b FROM C WHERE a = 10 ORDER BY b
```

This is the case where *interesting orders* comes into play. The paths here are - Full scan, then sort by b - Index scan on a then sort by b - Index scan on b (will already be sorted)

In this case even if the "a" index is lower cost than the "b" index, the cost has to included the extra sort step.

#### Joins

Joins get hard because you have to consider all of the stuff from the single relation case, but also the *order* in which joins occur, and also which type of join will be best.

Even if the query has the tables in a particular order, the planner might use a different order that's more efficient. The *result* is the same. If you have N tables in the FROM, there are N! factorial possible join orders, which is too many when N \> 3 or so.

> A heuristic is used to reduce the join order permutations which are considered. When possible, the search is reduced by consideration only of join orders which have join predicates relating the inner relation to the other relations already participating in the join

##### Join Types

Two types of join considered in the paper: nested loop, and merge join. Merge join takes advantange of indexes on join columns, which avoids needing to rescan entire inner relation. Hash joins weren't really a thing until the 1980s, primarily because they take a lot more memory.

##### Dynamic Programming

> The search tree is constructed by iteration on the number of relations joined so far. First, the best way is found to access each single relation for each interesting tuple ordering and for the unordered case. Next, the best way of joining any relation to these is found, subject to the heuristics for join order. This produces solutions for joining pairs of relations. Then the best way to join sets of three relations is found by consideration of all sets of two relations and joining in each third relation permitted by the join order heuristic

I think this is the "dynamic programming" approach? I think the cost in joins is always based on the scan of "outer" join plus the scan of the "inner", but the outer join might be the cumulative results of prior joins. It has the structure of a classic dynamic programming problem.

So the basic approach is that they build a tree of possible approaches for joining the tables, and then choose the "path" with the lowest cost. Most of the cost is again just cardinalities of the relations, but again sort orders and join methods have to be considered.

> The number of solutions which must be stored is at most 2\*\*n (the number of subsets of n tables) times the number of interesting result orders

Example from paper:

``` sql
SELECT NAME,TITLE,SAL,DNAME
FROM EMP,DEPT,JOB
WHERE TITLE=‘CLERK’
AND LOC=‘DENVER’
AND EMP.DNO=DEPT.DNO
AND EMP.JOB=JOB.JOB
```

In the join, they first look at scanning options for EMP, DEPT, and JOB.

Then they work on the join (EMP, DEPT), (EMP, JOB), (DEPT, EMP) and (JOB, EMP), but not (DEPT, JOB) or (JOB, DEPT) because there's no join predicate for them.

And so forth... a couple of details:

The approach used here is now called "left-deep" because it's grouped like ((A join B) join C) join D. There are apparently later optimizer that used "right-deep" and then something called "bushy trees", which are grouped like (A join B) join (C join D). The latter is easier to parallelize. Left-deep was apparently the best choice at the time because it was memory efficient.

The predicates on equijoins are also "interesting orders". Since these can extend across multiple tables, they are organized into "equivalence classes" (a term from set theory).

> To minimize the number of different interesting orders
> and hence the number of solutions in the tree, equivalence classes for interesting
> orders are computed and only the best solution for each equivalence class is saved.
> For example, if there is a join predicate
> E.DNO = D.DNO and another join predicate
> D.DNO = F.DNO then all three of these columns belong to the same order equivalence class.

##### Computational Complexity

Apparently JOIN ordering is NP-hard if you do the full N! set of possible orderings. So, the dynamic programming approach does not necessarily give the best possible answer even if the table/index statistics are perfect.

##### **The independence assumption**

> Many optimizers do not model highly correlated data really well. For example, 90210 is a zip code that’s only in California. Zip codes are not evenly distributed across states, and there isn’t a 90210 in every state of the union. For a user request, nailing down the zip code to 90210 is sufficient and applying another predicate, such as state equals California, doesn’t change the result. It won’t reduce the number of rows because the only 90210 is in California.

I think most CBOs still have this independence assumption for the most part, but i did read something recently about order dependence. In other words, if you have a predicate on some field that doesn't have an index, the optimizer might use a different index if that field has the same sort order as another field.

#### Connection to Current Optimizers

The CBO in System R went straight into IBMs DB2.

Cost-based optimizers are still pretty much the main thing being used in current RDMS. Postgres has a version of CBO, but also something called the [Genetic Query Optimizer (geqo)](https://www.postgresql.org/docs/current/geqo.html) that kicks in when there a lot of tables in a join.

A couple of modern versions of CBO came out in the 90s, the Volcano and then Cascades optimizer frameworks. SQL Server uses something called Calcite that's based on Volcano.

There were some papers around 2020/2021 talking about machine-learning based optimizers, but i don't know if any of them are in production.

##### Cardinality Estimation

An ongoing issue with CBOs is that they need good statistics on the tables and indexes. A paper from 2015 (How Good are Query Optimizers, Really?) talks about this and how bad cardinality estimates affect plans negatively.

> We have also shown that relational database systems produce large
> estimation errors that quickly grow as the number of joins increases,
> and that these errors are usually the reason for bad plans.

A recent paper that was in CACM this year talks about something called "Cardinality Estimation Graphs", so this is still a problem of interest.

------------------------------------------------------------------------

## The Seminal Paper

**"Access Path Selection in a Relational Database Management System"**\
Patricia G. Selinger, Morton M. Astrahan, Donald D. Chamberlin, Raymond A. Lorie, Thomas G. Price\
*Proceedings of ACM SIGMOD, 1979*

-   📄 [PDF (Duke CS)](https://courses.cs.duke.edu/compsci516/cps216/spring03/papers/selinger-etal-1979.pdf)

-   🔗 [Semantic Scholar](https://www.semanticscholar.org/paper/Access-path-selection-in-a-relational-database-Selinger-Astrahan/7def002796277facffe02aa09e3a1bb101ec0785)

-   🔗 [IBM Research entry](https://research.ibm.com/publications/access-path-selection-in-a-relational-database-management-system)

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