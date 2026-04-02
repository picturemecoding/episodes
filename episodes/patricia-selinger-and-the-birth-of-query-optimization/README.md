# Patricia Selinger and the Birth of Query Optimization

## Episode Overview

A deep dive into one of the most influential papers in database history: Patricia Selinger's 1979 SIGMOD paper "Access Path Selection in a Relational Database Management System." We explore how Selinger and the IBM System R team invented cost-based query optimization — a technique so sound that it underpins virtually every relational database in use today.

**Episode type:** Topic  
**Hosts:** Mike & Erik

---

## Background & Historical Context

### The Problem Before System R

In the early 1970s, relational databases were a theoretical idea — elegant on paper (thanks to Codd's 1970 relational model paper) but widely dismissed as impractical. The prevailing wisdom was that navigational databases (IMS, CODASYL) were the only way to get acceptable performance. One key skeptic argument: if you let users write high-level declarative queries (SQL), how would the system ever figure out an *efficient* way to execute them?

The System R project at IBM Research in San Jose (started ~1973) was the attempt to prove the skeptics wrong. It was one of the most consequential research projects in software history — it gave us SQL, it gave us transactions (ACID), and it gave us query optimization.

### Patricia Selinger Joins IBM (1975)

Patricia Griffiths Selinger received her A.B., S.M., and Ph.D. in **Applied Mathematics** from Harvard University (1971–1975) — notably *not* computer science. She joined IBM Research in San Jose the same year she finished her PhD and quickly became a central figure on the System R team.

Her background in applied math, with its emphasis on optimization and cost modeling, turned out to be exactly the right lens for the problem she'd tackle: how do you automatically choose the best execution plan for a query?

### The Challenge of Query Optimization

When a user writes:
```sql
SELECT name FROM employees, departments
WHERE employees.dept_id = departments.id
AND departments.location = 'New York'
```

There are many ways to execute this:
- Which table do you scan first?
- Do you use an index or a full scan?
- For a join of N tables, there are N! possible orderings — for 10 tables that's 3.6 million

Before Selinger's work, systems used **heuristics** — rule-of-thumb decisions baked in by programmers. The System R team wanted something better: a principled, *cost-based* approach.

---

## The Seminal Paper

**"Access Path Selection in a Relational Database Management System"**  
Patricia G. Selinger, Morton M. Astrahan, Donald D. Chamberlin, Raymond A. Lorie, Thomas G. Price  
*Proceedings of ACM SIGMOD, 1979*

- 📄 [PDF (Duke CS)](https://courses.cs.duke.edu/compsci516/cps216/spring03/papers/selinger-etal-1979.pdf)
- 🔗 [Semantic Scholar](https://www.semanticscholar.org/paper/Access-path-selection-in-a-relational-database-Selinger-Astrahan/7def002796277facffe02aa09e3a1bb101ec0785)
- 🔗 [IBM Research entry](https://research.ibm.com/publications/access-path-selection-in-a-relational-database-management-system)

### Core Ideas

#### 1. Cost-Based Optimization
Instead of hardcoded heuristics, the optimizer enumerates candidate execution plans and assigns each an *estimated cost* — measured in I/O operations and CPU time. It then selects the cheapest plan. Cost estimates are derived from **catalog statistics** (cardinality of tables, number of distinct values, etc.).

#### 2. Selectivity Estimation
For a predicate like `age > 30`, the optimizer estimates what fraction of rows will match — the **selectivity factor**. The paper introduced formulas still recognizable in modern systems:
- Equality predicate on a column with K distinct values → selectivity = 1/K
- Range predicate → selectivity = (max - value) / (max - min)
- Multiple predicates joined by AND → multiply selectivities (independence assumption)

This independence assumption is famously a source of error and has driven decades of follow-on research (histograms, sketches, ML-based cardinality estimation).

#### 3. Dynamic Programming for Join Ordering
Rather than exhaustive search (N! plans), the optimizer uses **dynamic programming**: build optimal plans for subsets of relations bottom-up, exploiting the principle of optimality. This reduces the search space from factorial to exponential — still expensive for very large joins, but tractable for the typical case.

System R focused specifically on **left-deep plans** (linear join trees where one input is always a base relation), which further constrains the search space and maps naturally to a pipeline execution model.

#### 4. Interesting Orders (the Subtle Genius)
This is perhaps the paper's most elegant insight. When evaluating plans, you don't just track the cheapest plan for each subset of relations — you also track the cheapest plan *for each "interesting ordering"* of the output.

An ordering is **interesting** if it's useful to some later operator: e.g., if the query has `ORDER BY dept`, or if a downstream join can exploit the sort. A plan that's slightly more expensive but produces sorted output may end up being globally optimal because it avoids a sort step later.

This idea — tracking a Pareto frontier of cost vs. output properties — is a precursor to modern concepts like physical properties in the Volcano/Cascades optimizer frameworks.

---

## Key Concepts for Discussion

- **Declarative vs. procedural queries**: why giving up control to the optimizer felt radical in 1979
- **The cost model**: what statistics System R maintained and how they estimated I/O vs. CPU costs
- **The independence assumption**: simple, wrong, and still everywhere — what's been done about it?
- **Left-deep vs. bushy join trees**: why System R restricted the search space and when that matters
- **Interesting orderings**: how this small insight has giant downstream effects
- **The optimizer as a compiler**: query optimization as a code generation problem
- **How the System R optimizer became DB2**: the paper's ideas were taken nearly verbatim into production

---

## Timeline

| Year | Event |
|------|-------|
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

---

## Discussion Questions

1. **On the leap of faith**: In 1979, trusting a machine to pick the best query plan felt radical. What had to be true about the state of computing for this to be accepted? Would it have been accepted earlier? Later?

2. **The math background angle**: Selinger came from applied mathematics, not CS. How much did that shape the cost-based framing vs. a more CS-heuristics approach?

3. **The independence assumption**: The paper assumes column predicates are independent when estimating selectivity. Every database researcher knows this is wrong. Why has it persisted for 45 years?

4. **Interesting orders as a design principle**: The idea of tracking "useful properties" of intermediate results beyond just cost is powerful. Where else in computing do we see this kind of multi-objective optimization?

5. **From research to product**: The System R optimizer went into DB2 "lock, stock, and barrel." How often does that happen? What made System R unusual?

6. **Modern query optimization**: With ML-based cardinality estimation (Bao, Neo, etc.) getting real traction in the 2020s, is the Selinger approach finally being superseded? Or augmented?

7. **The join ordering problem is NP-hard**: Modern systems cheat in various ways (heuristics above a join threshold, genetic algorithms, memoization). What are the tradeoffs?

---

## Interesting Stories & Angles

- **The skeptics were loud**: Mike Stonebraker was a prominent critic who argued relational databases would always be too slow. System R was a direct rebuttal. Selinger's optimizer was a key reason the performance story improved.

- **The paper almost didn't happen the way it did**: Selinger has described in interviews how the team debated whether to publish — IBM was protective of the work given its commercial value. The decision to publish at SIGMOD 1979 shaped the entire field.

- **The "interesting orders" naming**: The term itself is charmingly informal for something so mathematically precise. It's remained in the literature for 45+ years.

- **Harvard Applied Math → database internals**: A great "unlikely path" story — ask Mike and Erik to reflect on how often the deepest systems work comes from people with non-traditional CS backgrounds.

- **The ACM Queue interview (2006)**: Conducted by James Hamilton (later of AWS fame), Selinger is candid about the evolution of the optimizer, what she'd do differently, and the challenge of moving research to product. Worth quoting directly.

- **The CACM Database Dialogue (2008)**: Another rich interview where she reflects on 30+ years of database history and the state of the field.

---

## Further Reading

- [Access Path Selection paper (PDF)](https://courses.cs.duke.edu/compsci516/cps216/spring03/papers/selinger-etal-1979.pdf)
- [A Conversation with Pat Selinger — ACM Queue (2006)](https://queue.acm.org/detail.cfm?id=1059803)
- [Database Dialogue with Pat Selinger — CACM (2008)](https://cacm.acm.org/magazines/2008/12/3355-database-dialogue-with-pat-selinger/fulltext)
- [Pat Selinger Speaks Out — SIGMOD Interview (PDF)](https://sigmod.org/publications/interviews/pdf/17.selinger-interview.pdf)
- [Patricia Selinger — IBM History](https://www.ibm.com/history/patricia-selinger)
- [System R: Database Research Retrospective — TODS 1981](https://dl.acm.org/doi/10.1145/319996.319997)
- Graefe, G. (1995). *The Cascades Framework for Query Optimization* — direct intellectual successor

---

## Music Segment

_[Mike and Erik — what are you listening to?]_

---

*Research compiled: April 2026*
