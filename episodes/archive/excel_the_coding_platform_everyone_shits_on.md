# The Agony and the Ecstasy of Excel

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

## History of Excel (<5m)

- Spreadsheets formally showed up around 1909 as an accounting document
  + Comprehensive
  + Visible
- Versions of spreadsheet software have existed throughout computing, but had to be manually advanced until no calculations remained
- LANPAR was the first automatic-advance software that would proceed until no calculations remained
  + Execution Plan
- VisiCalc was the first WYSIWIG spreadsheet editor, followed by Lotus 1-2-3 and eventually Excel
- Excel introduced in 1985 (only for Macintosh). Windows version 2.0 came out two years later.
- Most popular version came out in 1992 (coincides with desktop abundance in companies). In 1993 they introduced VBA.
- Excel limits:
  + Total number of rows: 1,048,576 rows
  + Total columns on a worksheet: 16,384 columns
  + Page breaks: 1,026 horizontal and vertical
  + [Erik] There’s gotta be a financial limit too right? Like “only $4billion of company value is allowed in a single spreadsheet. Any more than that and you have to start a new spreadsheet.”

## Excel as a software deployment platform (a coding platform) (~15-20m)

- VBA (1993)
  + Other languages: M lang (powerbi language), Office Script (a Typescript thing, 2019), Python (2024…?)
  + Formula-languages: excel formulas (include lambdas and array functions, and Data Analysis Expressions (DAX) functions )
- Visual Basic for Applications OOP makes all this possible
  + Document handling
  + Event handling
  + Code execution (macros)
  + Custom formulas
- Functional programming language (from the podcast episode “Advancing Excel as a programming language with Andy Gordon and Simon Peyton Jones” from Mar 2021): [QUOTE] “Excel is the world’s most widely used functional programming language!” (A) (Here he’s talking about the lambdas and array functions)
- “With the introduction of LAMBDA, Excel became Turing complete.” (wikipedia)
- Justin: “I just read that like 94% of the excel sheets have fundamental flaws in the math in them.” (link from Justin: “[Study finds 94% of business spreadsheets have critical errors](https://phys.org/news/2024-08-business-spreadsheets-critical-errors.html)”)
- The original C++ code doesn’t use IEEE arithmetic believe it or not. It predates IEEE arithmetic.
- “Despite the use of 15-figure precision, Excel can display many more figures (up to thirty) upon user request. But the displayed figures *are not those actually used in its computations*, and so, for example, the difference of two numbers may differ from the difference of their displayed values. Although such departures are usually beyond the 15th decimal, exceptions do occur, especially for very large or very small numbers. Serious errors can occur if decisions are made based upon automated comparisons of numbers (for example, using the Excel If function), as equality of two numbers can be unpredictable”

## Excel as a low-code tool (~5m)

- Data-exploration (Bob’s observation)
- Code base is still expanding
- Expansion in the past 10 years breaks some of the formal barriers that limited usability
  + Spill formulas
  + Less cumbersome handling of arrays
  + filter() is so goddamn useful
- Graphing and Plotting
- Bob: the spreadsheet *is* the GUI. there i no need to build another gui on the top of your code. If the machine runs windows, it runs Excel => you have your entire application.
- Constant change is possible with the tools you have. You can reference the cell with some value(s) and then add a new sheet and riff on that till you get the information you want from that data.
- [QUOTE] Podcast 35:53: “I actually believe that the spreadsheet environment is a great way to learn because it’s so live. You make a change and you can immediately see results.” (D)

## The role of the excel worker. The credibility of the Excel skillset? (~5m)

## How do we rate Excel

## [QUOTE] “Not Programmers”:

podcast:

> “The kind of people who use excel are generally not programmers, people who really care about writing code for its own sake… Generally, they’re really keen to get some other job done… They’re what are known as end-user programmers…” (B)

## Excel as a database (~5m)

- Not a relational database, which is a performance bottleneck
- More agile than a relational database
- Pivot tables are introduced to aggregate sheet data
- Power Pivot / Power Query ends up replacing MS’s actual DB product (Access) in a lot of ways as desktop compute matures
- Excel as an endlessly consternating thing for software devs
- Still essentially a document editor
  + publishing /exporting programmatically is locked down
    - Good reason for this, all kinds of sensitive data live in spreadsheets
  + WYSIWIG introduces layout features that obscure and confuse
    - MERGED CELLS
    - Incel traits, can’t identify a date to save it’s life

## Should we build the business on top of Excel? (~15-30m)

- Google is pushing real hard in this direction <https://about.appsheet.com/home/>
  + Not yet workable at scale
- When does it go too far?? (What hath we wrought?!)

## References

- <https://phys.org/news/2024-08-business-spreadsheets-critical-errors.html>
- [Advancing Excel as a programming language with Andy Gordon and Simon Peyton Jones](https://www.microsoft.com/en-us/research/podcast/advancing-excel-as-a-programming-language-with-andy-gordon-and-simon-peyton-jones/)
- [Wikipedia](https://en.wikipedia.org/wiki/Microsoft_Excel)

From Bob:

Here are some things I have seen with Excel in finance in particular (after writing some of these, they all come back to “exploratory analysis” where you are not certain of the outcome and want to be able to pivot to doing different things when you have gone along part of the way)

1. You can “see the data” and know if you have removed it or deleted it or that it is incomplete.
   1. Being able to page through the data, look at the values and the cells, look at a particular row and see if something is not quite right. Many people REALLY value this. When a total in a numpy array is wrong, you have to do many things to find the errant value.
2. Plug-and-play new values (stealing from others)
   1. When your boss sets up a prototype of the calculation, you can just take the sheet, then you can copy the formulas down or unpack what it does. It is truly a no-code way to share the steps for an algorithm.
3. Integrated graph/plot (and updates immediately…)
   1. When trying to “figure something out” you can look at the data very easily with a variety of plots and then if you update values, the plots update immediately.
4. Forms and other ActiveX tools allow one to create click buttons that can page through data.
5. Macros allow you to record what you are doing, and then become an entry point to interact with VBA code on the back-end.
6. The spreadsheet *is* the GUI. there is no need to build another gui on the top of your code. If the machine runs windows, it runs Excel => you have your entire application.
7. Constant change is possible with the tools you have. You can reference the cell with some value(s) and then add a new sheet and riff on that till you get the information you want from that data.

However, I think we all have stories of when things have “gone too far” and they missed the migration to a more robust system. I have at least 3 of these I can think of right away.

Remember, all these cases are done with no source control. The minimum amount of money handled here is probably 100 million dollars. Likely quite a bit more but I didn’t calculate that at the time.

1. Spreadsheet replaces the paper “blotter” for trading records (becomes a “database” with no guardrails) -> same spreadsheet has some analytics added to show risk -> same spreadsheet then has “live quotes” to show risk and positions -> same spreadsheet has more “indicators” added. -> Same spreadsheet
2. Spreadsheet is setup to “pull in positions of the firm” (sometimes this is a manual load of positions from a CSV or e-mail) -> spreadsheet then calculates a Value-at-risk risk metric as a “prototype” for one portfolio -> spreadsheet then has graphs added to it to “show the risk” -> spreadsheet then becomes a hairball of macros that “automate the process” (including automated email of the spreadsheet itself!) -> entire “workflow” is presented to federal auditors as “solution to firm-wide risk calculation” for a major bank.
3. Think of the book/movie The Big Short - they are trading mortgages. There was a group (it was two people) that were trading a very large mortgage portfolio. It grew from a small spreadsheet where they tried to price individual “pools” of mortgages. They developed this spreadsheet solution over the course of probably 2 years in total. They got some data, put it into a spreadsheet and did “analysis” to find the cheap ones to buy. That mostly worked, so they wanted to process more data (but not depend on other people…) Then the process became automated download of generally available data (from an FTP server, once a month) and the “processing” of this data in the same spreadsheet where it would use macros and open files, copy ranges to other ranges, close the files and open new files. Then it would do a series of groupings, pivots, calculations and the whole process would take 2-4 hours when you launched it (usually late in the day or overnight) Then you would get an output (which was a z-score) for all the input “pool” data and they would use this output to trade huge positions. The group “really liked that they could see all the data and make changes when needed.” but when it broke, only those two people could “fix” it and neither was a professional programmer. I spent about 9 months trying to move this to an access database for the automated load an analysis. In they end they didn’t use it because they “could not see all the data and make changes” !!