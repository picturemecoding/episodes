# Relational Databases

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

Mention two weeks thing…

Mike Music: From the Private Collection of Saba and No ID

Erik Music: Heems *Lafandar*. I love this record so much. I can’t remember another record I’ve heard this year that I’ve loved this much.

On Yellow Chakra, he raps “I'm like a city during COVID, said my bars infectious” and then later Open Mike Eagle says, “I’m track three from It was Written.” I’ve never heard a flex like that; it’s totally bitchin. The song Porches is so good, reminds me of something crystal clear sounding from 90s hip hop. On “Bukayo Saka” he references the Mabharata and hten says this hilarious thing “I roll around with two glizzies like I'm Slavoj Zizek,” which is about this video of the cultural critic slavoj zizek walking around with a hot dog in each hand.

## Discussion

Our first episode in our database series (figuring the Jim Gray episode was episode 0) will discuss relational databases. Gray himself contributed primarily to relational systems, and the leader of his group at IBM was E.F. Codd, the person who essentially invented the idea.

My own first exposure to relational databases was, somewhat strangely, Postgres in around 1992 or 1992. At the time it was a fairly experimental system and it was trying to add object-oriented features (eg, fields that would compute values from other fields). However, it was architecturally relational and borrowed ideas from its predecessor, Ingres.

I did not use relational databases on a regular basis until the late 90s, specifically Oracle. At that time Oracle was the main commercial option, although DB2 and Ingres were both widely used in large businesses. These systems were primarily used for storing business data and generating reports with SQL and PL/SQL. Using them from a desktop or web application was the less common use case, and by this time i think a lot of web apps were already moving to MySQL.

## What is a database?

- IBM: A database is a digital repository for storing, managing and securing organized collections of data.
- AWS: A database is an electronically stored, systematic collection of data. It can contain any type of data, including words, numbers, images, videos, and files. You can use software called a database management system (DBMS) to store, retrieve, and edit data. In computer systems, the word database can also refer to any DBMS, to the database system, or to an application associated with the database.
- Mongo: Formally, a database is an organized collection of structured or unstructured information stored electronically on a machine locally or in the cloud. Databases are managed using a Database Management System (DBMS). The DBMS acts as an interface between the end user (or an application) and the database. Databases use a query language for storing or retrieving data.

### What Makes a DBMS Relational?

- Data is stored as relations (tables, rows, columns)
  + Ordering of rows is arbitrary
  + Ordering of columns is important
- Some subset of columns is unique across all rows (primary key)
- Indexes are independent of data (ie, you can add/remove indexes without affecting how you access the data)
- Common operations (relational algebra)
  + Projection
  + Joins
  + Restriction (Filtering)
- Special language for querying (usually SQL)
- Complex data stored in normal form
  + Relationships via foreign keys
- What about the Sequent Calculus? (Am I misremembering here? What’s the name of the abstractions which talk about “projections” and something else… “relational algebra”?)
- What about the ANSI SQL Standard?

### Who Uses RDBMSs?

The Relational Database Management System is still the predominant database technology. The well known players are Oracle, MySQL, SQL Server, Postgres, and variations of those (RDS and managed systems on other cloud providers).

The Relational Database Software Market size was estimated at USD 21.97 Billion in 2024 and is projected to reach USD 45.23 Billion by 2031,

RDMBSs are about 62% of the overall database market

MySQL still has the largest market share (41%)

[Best Relational Databases Software in 2025 | 6sense](https://6sense.com/tech/relational-databases)

## History

[Dr. Michael Stonebraker: A Short History of Database Systems - The New Stack](https://thenewstack.io/dr-michael-stonebraker-a-short-history-of-database-systems/)

Stonebreaker:

in the 1970s, there was only one database market, basically business data processing. And the whole goal of data management was to make business databases work better. Relational databases were originally designed with that goal in mind, and that was the only market anyone really saw until around 1990. Then I think over the next 15 years, it occurred to most everybody that they needed a database system.

[A Timeline of Database History | Quickbase](https://www.quickbase.com/articles/timeline-of-database-history)

### Prehistory

Before the relational DB there were network and hierarchical databases.

Network database (CODASYL/Bachman)

Hierarchical database (IMS)

### 1970

E.F. Codd

[A Relational Model of Data for Large Shared Data Banks](https://www.seas.upenn.edu/~zives/03f/cis550/codd.pdf)

The term *relation* is used here in its accepted mathematical sense. Given sets S1 , S2, , . . . , Sn, (not necessarily distinct), R is a relation on these n sets if it is a set of n-tuples each of which has its first element from S1, its second element from Sz , and so on.’ We shall refer to Sj as the jth domain of R. As defined above, R is said to have degree n. Relations of degree 1 are often called unary, degree 2 binary, degree 3 ternary, and degree n n-ary.

More concisely, R is a subset of the cartesian product S1 X S2 … X Sn

Specifically, an *n-ary relation*.

Efficiency?

Ted Codd wrote his pioneering paper in 1970 that said you should view data management as tables, the simplest possible data structure, and then access them in a high-level language. Now that means SQL. These were revolutionary thoughts at the time and went counter to all the existing data management systems.

Immediately, there was a huge debate between the relational folks who said Ted Codd’s ideas looked great, and the traditionalists who said you can’t possibly build one of these to be efficient– and even if you could, no one could understand these newfangled languages.

### 1973

System R

Ingres (Stonebraker/Wong, Berkeley)

First appearance of SQL

In 1973 when the System R project led by Edgar Codd was getting started at IBM, the research team released a series of papers describing the system they were building.[8] Two scientists at Berkeley, Michael Stonebraker and Eugene Wong, became interested in the concept after reading the papers, and started a relational database research project of their own

Chamberlin and Boyce's first attempt at a relational database language was SQUARE (Specifying Queries in A Relational Environment), but it was difficult to use due to subscript/superscript notation. After moving to the San Jose Research Laboratory in 1973, they began work on a sequel to SQUARE.[12] The original name SEQUEL, which is widely regarded as a pun on QUEL, the query language of Ingres,[14] was later changed to SQL (dropping the vowels) because "SEQUEL" was a trademark of the UK-based Hawker Siddeley Dynamics Engineering Limited company.[15] The label SQL later became the acronym for Structured Query Language

### 1979

Oracle (first commercial RDBMS?)

### 1981

Ingres commercialized

### 1982

IBM DB2

Despite all the research they did, IBM was a bit late to the market.

### 1986

Postgres

[Documentation: 17: 2. A Brief History of PostgreSQL](https://www.postgresql.org/docs/current/history.html)

[THE DESIGN OF POSTGRES Abstract 1. INTRODUCTION 1](https://dsf.berkeley.edu/papers/ERL-M85-95.pdf)

<https://dsf.berkeley.edu/papers/ERL-M87-13.pdf>

From the Stonebreaker interview:

And to the first approximation, relational databases fell on their face when you tried to apply them in different areas. The problem wasn’t the relational model, it was more the data types that INGRES and System R supported were floats, integers, character strings, money, and that’s what the business people wanted. But if you want to build a geographic information system, you want points, lines, polygons, that sort of stuff.

So one of the basic ideas in Postgres was let the user have whatever basic data types he wants to manage, and don’t predefine them by insisting they be the ones that apply to business data processing.

SQL becomes ANSI standard

### 1989

Microsoft SQL Server 1.0

### 1995

MySQL (did you know that the dolphin in the logo has a name? “Sakila”)

### 2000

SQLite

### 2008

Sun buys MySQL AB

### 2009

AWS RDS (MySQL)

### 2010

Oracle buys Sun

MariaDB gets forked