# the_history_of_unix

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

Erik Music: Karma to Burn *Appalachian Incantation* and *Karma to Burn* (1997)

Mike Music: Rick Beato’s Youtube Channel

## Discussion Outline

### Introduce source material for today

- Brian Kernighan’s *Unix: A History and a Memoir* (2019?)
- Dennis Ritchie and Ken Thompson: “The Unix Time-sharing system” (1974)
- A bit from John Lions Annotated version 6 Unix: *A Commentary on the Unix Operating System*

### Introduce Unix and Bell Labs

Unix is an operating system created at Bell Labs in the 70s which started to become widely used at universities in the 70s and then at some businesses in the 80s. The design of the operating system as well as the various tools included in Unix and the C programming language are now so widespread as to be pervasive.

Although Unix was the most visible software from Bell Labs, it was by no means the only contribution to computing. The Computing Science Research Center, the fabled “Center 1127,” or just “1127,” was unusually productive for two or three decades.

They produced all kinds of amazing contributions to computing. They invented the *transistor* and the original idea was, “hey, this will allow us to make better radar systems and smaller radios!”

Other Bell Labs Contributions

- Claude Shannon’s Information Theory
- Transistors
- Richard Hamming and error-correcting Codes
- Early networking technology
- Unix
- The C programming language
- Bjarne Stroustrup wrote C++ there
- Plan 9: unicode
- Cosmic Microwave Background radiation

Unix inspired BSD and Linux, which by far are the most widely deployed operating systems on the planet. As software devs, I still use, dozens of times a day, tools in my terminal that they created at Bell Labs or I use a descendant.

#### Unixy Things You May Have Used

- Shell stuff: ls, mv, cd, wc
- diff
- Grep
- Who,
- Yacc, Lex
- Bash (7th edition)
- Make
- Ed, Awk, Sed
- Socket (contributed by Bill Joy who started Sun and wrote vi, [BK 135]

## Unix Timeline & Origin Story

- CTSS: Compatible Time-Sharing System, created at MIT in 1964
  + In contrast to batch processing with punch cards, CTSS programmers “used typewriter-like devices (‘terminals’) that were connected directly or by phone lines to a single big computer… The operating system divided its attention among the users who were logged in, switching rapidly from one active user to the next, giving each the illusion that they had the whole computer at their disposal.”
- CTSS was so good that researchers at MIT wanted to create a better one, so in 1965, they started designing a new OS called “Multics” (Mutiplexed Information and Computing Service). This was a huge project, so they brought in General Electric and Bell Labs to help.

Brian Kernighan says they had around half a dozen people from Bell Labs working on Multics, including Doug McIlroy, Ken Thompson, and Dennis Ritchie, and their experience paved the way for Unix. Multics actually had a dedicated programming language for writing Multics software: PL/I.

Multics was ambitious and hard, and it soon ran into problems. In retrospect, it was partly a victim of the *second system effect*. My favorite line about Multics from Kernighan’s book is:

The phrase “over-engineed” appears in several descriptions and Sean Morgan described it as “an attempt to climb too many trees at once”. (p 32)

Around this time, Ken found a little-used DEC PDP-7, a computer whose main purpose was as an input device for creating electronic circuit designs.

The PDP-7 was ﬁrst shipped in 1964 and computers were evolving quickly, so by 1969 it was dated. The machine itself wasn’t very powerful, with only 8K 18-bit words of memory (16K bytes).

Kerhnighan writes that the PDP-7 had this very tall disk drive, but “the disk was too fast for the computer.” So Ken Thompson writes a disk scheduling algorithm to maximize disk throughput, but he needed some way to test it, which meant he needed to load data on it in large quantity.

“At some point I realized that I was three weeks from an operating system.” He needed to write three programs, one per week: an editor, so he could create code; an assembler, to turn the code into machine language that could run on the PDP-7; and a “kernel overlay—call it an operating system.”

Right at that time, Ken’s wife went on a three-week vacation to take their one-year-old son to visit Ken’s parents in California, and Ken had three weeks to work undisturbed.

I have a memory of standing in my ofﬁce doorway, talking with a group that probably included at least Ken, Dennis and Peter Neumann. The system had no name at that point, and (if memory serves) I suggested, based on Latin roots, that since Multics provided “many of everything” and the new system had at most one of anything, it should be called “UNICS,” a play on “uni” in place of “multi.

BK has a timeline copied from Wikipedia of Unix and its descendants [154].

### Cost and Effort

Perhaps the most important achievement of Unix is to demonstrate that a powerful operating system for interactive use need not be expensive either in equipment or in human effort. UNIX can run on hardware costing as little as $40,000, and less than two man years were spent on the main system software.

[It was first created on a PDP-7 and at the time they published this paper it ran only on PDP-11s]

Cost

A funny note on the cost, after Ken Thompson made it and their lab got excited about working on it, they asked their manager for a PDP-10, which “had a great deal more horsepower than the PDP-7.” Kernighan writes that the proposal to buy a PDP-10 was “for half a million dollars.” However, “the Multics experience was an all-too-recent bad memory, so the PDP-10 proposal never got off the ground.” The management position at the time also was “we don’t do operating systems” (Ken Thompson quote).

So then they made another proposal for a PDP-11, which would cost only $65,000 (compared with $500,000, this seems like a steal), but this was also rejected. So a guy named Joe Ossanna came up with a clever work around:

The Patent department would use a PDP-11 for preparing patent applications; the Unix group would write the necessary software for them, complete with a formatting program that would print applications in the proper format; and no, no one would be working on operating systems.

Effort

When Ken Thompson is talking about “man-years” in that quote above, he’s talking about version 3. Look at what the John Lions says in his *A Commentary on the Unix Operating system*, which is on version 6:

From the Lions Annotated Unix:

The amount of effort to write UNIX, while not inconsiderable in itself (~10 man years up to the release of the Level Six system) is insignificant when compared to other systems. (For instance, by 1968, OS/360 was reputed to have consumed more then five man millennia and TSS/360, another IBM operating system, more than one man millennium.) Of course there are systems which are easier to understand than UNIX but, it may be asserted, these are invariably much simpler and more modest in what they attempt to achieve. As far as the list of features offered to users is concerned, UNIX is in the “big league”. (Chapt 1)

From the Lions Annotated Unix:

Not least amongst the charms and virtues of the UNIX Time-sharing System is the compactness of its source code. The source code for the permanently resident “nucleus” of the system when only a small number of peripheral devices is represented, is comfortably less than 9,000 lines of code. It has often been suggested that 1,000 lines of code represents the practical limit in size for a program which is to be understood and maintained by a single individual. Most operating systems either exceed this limit by one or even two orders of magnitude, or else offer the user a very limited set of facilities, i.e. either the details of the system are inaccessible to all but the most determined, dedicated and long-suffering student, or else the system is rather specialised and of little intrinsic interest. (Preface, p.1)

~~~~

## Part 2

### What was Interesting or New About Unix

“The most important job of UNIX is to provide a file system.” The first edition had about 30 system calls, Kernighan tells us, and about half of these were related to the file-system.

Everything is a file! “Special files constitute the most unusual feature of the UNIX file system. Each I/O device supported by UNIX is associated with at least one such file.”

John Lions writes that the Unix OS (6th ed) code has the following “major functions” and everything else is utilities/tools:

• initialisation;

• process management;

• system calls;

• interrupt handling;

• input/output operations;

• file management.

Stdin, Stdout, stderr (which was added to build pipes).

“Implementation of the file system” 37.4 -> I-nodes section (p. 363)

“Images and processes” 37.5 (364)

Pipes

From the paper, they write “an extension of the standard I/O notation is used to direct output from one command to the input of another. A sequence of commands separated by vertical bars causes the Shell to execute all commands simultaneously and to arrange the standard output of each command be delivered to the standard input of the next command in the sequence”.

There is a painful example here in the paper of how you would run a series of programs that each uses the output of the previous ones in order to produce an error and it looks awful. It also struck me how it never occurred to me that without pipes you’d always have been forced to do that.

My personal favorite grep story comes from a day in 1972 when I was called by someone at the Labs who said

“I noticed that when I hold my new pocket calculator upside down, some of the numbers make letters; for example, 3 becomes E and 7 becomes L. I know you guys have a dictionary on your computer. Is there any way you can tell me what words I can make on my calculator when I hold it upside down?”

From the book

“Pipes are perhaps the single most striking invention in Unix” Kernighan says.

Doug McIlroy had the idea first in a 1964 paper. He wanted “to allow arbitrary connections in a sort of mesh of programs, but it was not obvious how to describe an unconstrained graph in a natural way… But Doug continued to nag and Ken continued to think.” [BK, 68]

[until] Ken is quoted as saying, “one day I got this idea” and he adds a pipe system call to the operating system in an hour. He described it as “super trivial” using the input/output redirection that was already present. So then he tries it out and calls it “mind-blowing”.

Pipes are fundamentally about function composition. I don’t remember grasping the idea of pipes, but they’re never far from what I’m doing in a shell.

“Ken and Dennis upgraded every command on the system in a single night.” [BK, 69]

Shell

- Allows users to run programs and interact with file system
- Wildcards (globbing)
- Input, output redirection
- Just another program (not something special) so it’s totally *replaceable* (see various shells: csh, fish, zsh)
- Allows backgrounding tasks instead of waiting
- Allows running a shell as a command which means you can write shell scripts as input and run them in the shell.

#### Unixy Things You May Have Used….

- Shell stuff: ls, mv, cd, wc
- diff
- Grep
- Who,
- Yacc, Lex
- Bash (7th edition)
- Make
- Ed, Awk, Sed
- Socket (contributed by Bill Joy who started Sun and wrote vi, [BK 135]

### Why Was It Successful?

Technical Successes

“Perhaps paradoxically, the success of UNIX is largely due to the fact that it was not designed to meet any predefined objectives.”

“First, since we are programmers, we naturally designed the system to make it easy to write, test, and run programs.”

The C programming language: “one of C’s novel contributions to programming languages was the way that it supported arithmetic operations on typed pointers. A pointer is a value corresponding to an address…” [BK, 77]. Ken Thompson tried converting Unix from Assembly to C three times in 1973 but it was too difficult BK says until Dennis added structs to C. So the 6th edition kernel has about 9,000 lines of C.

“The hierarchical file system was a major simplification of existing practice, though in hindsight it seems utterly obvious–what else would you want? … Filenames are simply the path from the root, with components separated by slashes.” [BK, 166]

Also, “Files contained uninterpreted bytes; the system itself does not care what the bytes are, or know anything about their meaning.” [BK, 166]

Finally, “files are created, read, written, and removed with half a dozen straightforward system calls.” [BK, 166]

BK says that a high-level implementation language for user programs *and* the OS is what helped Unix take off. He says other OSs tried this but “C was much suitable than its predecessors and it led to *portability* of the OS.” [BK, 167]

He also credits user-programmable shell, pipes, and programs as tools as part of its success. These all look like what Ken Thompson and Dennis RItchie said in the paper:

Cultural Successes

The Unix philosophy as stated by Doug McIlroy from 1978:

1. Make each program do one thing well.
2. Expect the output of every program to become the input to another, as yet unknown, program.
3. Design and build software, even operating systems, to be tried early, ideally within weeks. Don’t hesitate to throw away the clumsy parts and rebuild them.

Bell Labs cultural practices:

- Collegiality: Doug McIlroy said, “Collegiality was the genius of the system. Nobody’s advancement depended on the relationship with just one boss.”
- A Stable environment: money, resources, mission, structure, no thrashing!
- A problem-rich environment: As Dick Hamming said, “if you don’t work on important problems, it’s unlikely that you’ll do important work.” [BK 171] (ouch?)
- Technical managers: “the managers have to understand the work they manage” [BK 172]
- Subversive office personnel

### Subversive Office Personalities

- Working on operating systems as secret side quest while producing formatting software for the patent department
- The used their Votrax voice synthesizer and a program Doug McIlroy wrote to convert English phonemes into spoken sounds in a program called *speak* and then every day at 1pm the Vortrax would say “Lunchtime, lunchtime, lunchtime. Yummy, yummy, yummy.” (because the cafeteria closed at 1:15pm)
- Badges: Gremlin badge and Kernighan’s Mickey Mouse badge
- Peter Weinberger’s (department head for 1127) face on the water tower

## Quotes from the Paper

Perhaps the most important achievement of Unix is to demonstrate that a powerful operating system for interactive use need not be expensive either in equipment or in human effort. UNIX can run on hardware costing as little as $40,000, and less than two man years were spent on the main system software.

[At this time it ran only on PDP-11s]

“The most important job of UNIX is to provide a file system.”

“Special files constitute the most unusual feature of the UNIX file system. Each I/O device supported by UNIX is associated with at least one such file.”

“Perhaps paradoxically, the success of UNIX is largely due to the fact that it was not designed to meet any predefined objectives.”

“First, since we are programmers, we naturally designed the system to make it easy to write, test, and run programs.”

## Quotes from Book

Finally, although Unix was the most visible software from Bell Labs, it was by no means the only contribution to computing. The Computing Science Research Center, the fabled “Center 1127,” or just “1127,” was unusually productive for two or three decades

As Doug McIlroy said, “Collegiality was the genius of the system. Nobody’s advancement depended on the relationship with just one boss.”

The most innovative operating system of the time was CTSS, the Compatible Time-Sharing System, which was created at MIT in 1964.

In 1965, they began to design a system called “Multics,” the Multiplexed Information and Computing Service.

Multics was intrinsically a challenging prospect, and it soon ran into problems. In retrospect, it was partly a victim of the second system effect:

Around this time, Ken found a little-used DEC PDP-7, a computer whose main purpose was as an input device for creating electronic circuit designs.

The PDP-7 was ﬁrst shipped in 1964 and computers were evolving quickly, so by 1969 it was dated. The machine itself wasn’t very powerful, with only 8K 18-bit words of memory (16K bytes),

“At some point I realized that I was three weeks from an operating system.” He needed to write three programs, one per week: an editor, so he could create code; an assembler, to turn the code into machine language that could run on the PDP-7; and a “kernel overlay—call it an operating system.”

Right at that time, Ken’s wife went on a three-week vacation to take their one-year-old son to visit Ken’s parents in California, and Ken had three weeks to work undisturbed.

I have a memory of standing in my ofﬁce doorway, talking with a group that probably included at least Ken, Dennis and Peter Neumann. The system had no name at that point, and (if memory serves) I suggested, based on Latin roots, that since Multics provided “many of everything” and the new system had at most one of anything, it should be called “UNICS,” a play on “uni” in place of “multi.”

Ken was also an avid pilot, and regularly ﬂew himself and guests around New Jersey, starting from the airport in Morristown. He got other members of 1127 interested in ﬂying as well, and at peak there were half a dozen private pilots in the “1127 air force.”

My personal favorite grep story comes from a day in 1972 when I was called by someone at the Labs who said

“I noticed that when I hold my new pocket calculator upside down, some of the numbers make letters; for example, 3 becomes E and 7 becomes L. I know you guys have a dictionary on your computer. Is there any way you can tell me what words I can make on my calculator when I hold it upside down?”

"The 6th edition kernel has about 9,000 lines of C and about 700 lines of assembly language for machine-speciﬁc operations like setting up registers, devices and memory mapping."

## Quotes from Lions Book

- <https://warsus.github.io/lions-/>
- <https://cs3210.cc.gatech.edu/r/unix6.pdf>

1. Not least amongst the charms and virtues of the UNIX Time-sharing System is the compactness of its source code. The source code for the permanently resident “nucleus” of the system when only a small number of peripheral devices is represented, is comfortably less than 9,000 lines of code. It has often been suggested that 1,000 lines of code represents the practical limit in size for a program which is to be understood and maintained by a single individual. Most operating systems either exceed this limit by one or even two orders of magnitude, or else offer the user a very limited set of facilities, i.e. either the details of the system are inaccessible to all but the most determined, dedicated and long-suffering student, or else the system is rather specialised and of little intrinsic interest. (Preface, p.1)
2. The amount of effort to write UNIX, while not inconsiderable in itself (~10 man years up to the release of the Level Six system) is insignificant when compared to other systems. (For instance, by 1968, OS/360 was reputed to have consumed more then five man millennia and TSS/360, another IBM operating system, more than one man millennium.) Of course there are systems which are easier to understand than UNIX but, it may be asserted, these are invariably much simpler and more modest in what they attempt to achieve. As far as the list of features offered to users is concerned, UNIX is in the “big league”. (Chapt 1)

He writes that the Unix OS code has the following “major functions” and everything else is utilities/tools:

• initialisation;

• process management;

• system calls;

• interrupt handling;

• input/output operations;

• file management.

The high page of physical memory is reserved for various special registers associated with the processor and the peripheral devices. By sacrificing one page of memory space in this way, the PDP11 designers have been able to make the various device registers accessible without the need to provide special instruction types.

The method of assignment of addresses to registers in this page is a black art: the values are hallowed by tradition and are not to be questioned. (section 2.14, special devi registers)

The software interrupt (or “signal” is a mechanism for communication between processes, particularly when there is “bad news”. (section 4.8)