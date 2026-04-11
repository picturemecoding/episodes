# Regular Expressions

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

Mike music: Mark Hollis - Mark Hollis (1998). The front man from Talk Talk.

Erik Music: Brent Hinds from Mastodon died in a motorcycle accident. I’ve been relistening to *the Hunter*.

## Main Topic

Regular expressions (aka, regex) are interesting because they have deep computer science origins but also they’re just nifty and useful. We use them with grep, we use them in our applications (eg, the python regex package).

Also, they have a weird name. What does “regular expression” mean? It doesn’t really say much about the way that we use them as programmers.

They are also a good way to get yourself in trouble, because they can be complex and essentially impossible to debug. Programmer lore says that you should never attempt to use regexes for things like addresses, HTML, or even URLs, but why?

## What do we know about regexes informally?

- Text search and replacement
- People find them confusing
- It’s like another programming language, short and inscrutable
- Sometimes they can be “slow”? (backtracking can have exponential runtime!) and you can attack websites with them or have your site attacked:
  + SeeOWASP page on [regex Denial-of-Service attacks](https://owasp.org/www-community/attacks/Regular_expression_Denial_of_Service_-_ReDoS).
  + “A Regex pattern is called Evil Regexif it can get stuck on crafted input.” See also “catastrophic backtracking”.
  + There was a [CVE in the .NET framework in 2015](https://blog.malerisch.net/2015/09/net-mvc-redos-denial-of-service-vulnerability-cve-2015-2526.html) where they were using regexes to parse email, phone, and url

## Regex memes

1. “Now you have two problems” (Jeff Atwood [commentary](https://blog.codinghorror.com/regular-expressions-now-you-have-two-problems/) from 2008, [research/background](https://regex.info/blog/2006-09-15/247) on the quote)
   1. Should we talk about Perl? (or unix, linux?); it appears in relation to the first quote; read the research/background article.
2. xkcd “hero” cartoon (this one? [xkcd: Regular Expressions](https://xkcd.com/208/))
3. Parsing html with regexes on [stackoverflow](https://stackoverflow.com/questions/1732348/regex-match-open-tags-except-xhtml-self-contained-tags/1732454#1732454)
   1. “HTML establishes a breach between this world and the dread realm of c͒ͪo͛ͫrrupt entities (like SGML entities, but more corrupt) a mere glimpse of the world of reg​ex parsers for HTML will ins​tantly transport a programmer's consciousness into a world of ceaseless screaming”

## Origins of

[Regular expression - Wikipedia](https://en.wikipedia.org/wiki/Regular_expression)

Things to think about:

- Kleene/regular languages
- Deterministic Finite Automata (DFA) and NFA
- Context-free languages/grammars

The original paper where Kleene talked about regular languages and created Kleene's theorem was improbably called "Representation of Events in Nerve Nets and Finite Automata"

[Representation of Events in Nerve Nets and Finite Automata](https://www.rand.org/content/dam/rand/pubs/research_memoranda/2008/RM704.pdf)

## Discursive Foray into Nerve Nets and Mcculloch and Pitts

- + McCulloch and Pitts were logicians who tried to develop a theory of how the mind worked based on a primitive neural network model.
  + [The Man Who Tried to Redeem the World with Logic - Nautilus](https://nautil.us/the-man-who-tried-to-redeem-the-world-with-logic-235253/)

## Back to Kleene

- Regular languages. Kleene’s Theorem basically says that these are the same:
  + A regular language (given by a regular expression)
  + A language that is accepted by a non-deterministic finite automaton
  + A language that is accepted by a DFA

From the paper:

We shall presently describe a class of events which we will call "regular events." (We would welcome any suggestions as to a more descriptive term.\*) Our objective is to show that all and only regular events can be represented by nerve nets or finite automata

DFA and NFA are finite state machines. From wikipedia on [finite state machine](https://en.wikipedia.org/wiki/Finite-state_machine):

Finite-state machines can be subdivided into acceptors, classifiers, transducers and sequencers.

Acceptors (also called detectors or recognizers) produce binary output, indicating whether or not the received input is accepted.

A (possibly infinite) set of symbol sequences, called a formal language, is a regular languageif there is some acceptorthat accepts exactly that set.

### DFA

A DFA is a type of state machine, obviously with a finite number of states. It is deterministic in the sense that given a particular symbol in a particular state it always transitions to the same new state. You can turn one of these into an NFA, which makes sense, I think. Contrasted with NFA…

### NFA

An NFA is different in that for a given symbol it can translate to different states, usually with some probability (people sometimes use “transitions graphs” to represent NFA)

An NFA can also [be turned *into* a DFA](https://en.wikipedia.org/wiki/Powerset_construction)…

- A DFA keeps track of a single set of states: deterministic state transitions
- An NFA keeps track of *all* state transitions, but the resulting answer is a single set of states, a DFA?

### Context-Free Languages and Pushdown Automata

Context-free languages require something more complicated (pushdown automata). Recursive structures, e.g., things with parentheses are context-free, as is a^nb^n (i.e. same number of a’s and b’s, because a “memory” is needed to keep track of the number of a’s).

Regular languages are context-free languages, but not vice-versa

A lot of what we call regex matching isn’t truly regex, for example the backreferences in Python regexes are context-free.

Context sensitive languages (other than natural language) are things like a^nb^nc^n or programming languages (eg, a statement can’t be evaluated unless the variables are declared)

A “Pushdown Automaton” has a stack and can compute things that require a stack. You can only push and pop from a stack, so this is different from a Turing machine (or a full computer) where you can access arbitrary memory.

The stack offers *only the top* when the Automaton is considering its next state transition.

From [Wikipedia on Pushdown Automaton:](https://en.wikipedia.org/wiki/Pushdown_automaton)

![](data:image/png;base64...)

## History and Variations

- McCulloch and Pitts and nerve nets (30s-40s)
- Kleene’s Theorem, regular languages, automata theory in 50s
- Ken Thompson builds regexes into QED (a text editor) in the 60s
- Above puts them into ed in Unix in the 70s
  + There is an [IEEE Posix Standard](https://en.wikipedia.org/wiki/Regular_expression#POSIX) for Regexes
  + “For example, GNU grep has the following options: "grep -E" for ERE, and "grep -G" for BRE (the default), and "grep -P" for Perl regexes.” ([wikipedia](https://en.wikipedia.org/wiki/Regular_expression#POSIX))
- Henry Spencer adds regexes to Perl and TCL in the 80s (and this is the form of regexes that appeared in many languages and other software projects, such as Postgresql)
- 1997: PCRE (Perl-compatible regular expressions)
  + Appears in APache, Nginx, Perl 6, R, PHP

## Summary of Automata Theory

Imagine we can build four different robots:

1. One with no stack or memory whatsoever where each input has a single outcome [DFA]
2. One with no stack or memory whatsoever where each input has multiple potential outcomes [NFA]
3. A robot like the above but it can consult a stack to help keep track of state [pushdown automaton]
4. A robot like the above with stack and heap [turing machine, computer]

The question then is: which types of problems are suited to which type of robot?

What are some examples:

- A coffee machine with a timer
- An elevator (a DFA or an NFA? What if it gets two button presses on different floors at the same time?)
- PDA: Parsing programming languages, text-editors with an undo/redo system, XML, HTML, or JSON validation. Information-processing systems(I couldn’t think of any real-world systems because why not just give your robot a computer brain if it needs *computation*?)

## References

- [Regular expression - Wikipedia](https://en.wikipedia.org/wiki/Regular_expression)
- Jeff Atwood 2008 [Commentary](https://blog.codinghorror.com/regular-expressions-now-you-have-two-problems/)
- History, [research/background](https://regex.info/blog/2006-09-15/247) on Jamie Zawinski’s “now you have 2 problems”
- [xkcd: Regular Expressions](https://xkcd.com/208/)
- Famous StackOverflow post on parsing HTML with regexes: [stackoverflow](https://stackoverflow.com/questions/1732348/regex-match-open-tags-except-xhtml-self-contained-tags/1732454#1732454)
- OWASP Regexes DOS: [regex Denial-of-Service attacks](https://owasp.org/www-community/attacks/Regular_expression_Denial_of_Service_-_ReDoS)
- McCullough and Pitts Nautilus Article: [The Man Who Tried to Redeem the World with Logic - Nautilus](https://nautil.us/the-man-who-tried-to-redeem-the-world-with-logic-235253/)
- Kleene’s paper [Representation of Events in Nerve Nets and Finite Automata](https://www.rand.org/content/dam/rand/pubs/research_memoranda/2008/RM704.pdf)
- Wikipedia [Powerset Construction](https://en.wikipedia.org/wiki/Powerset_construction) (turning NFA into a DFA)
- Wikipedia: [inite state machine](https://en.wikipedia.org/wiki/Finite-state_machine)
- Wikipedia: [push-down automaton](https://en.wikipedia.org/wiki/Pushdown_automaton)
- Catastrophic Regex in the [.NET Framework in 2015](https://blog.malerisch.net/2015/09/net-mvc-redos-denial-of-service-vulnerability-cve-2015-2526.html) when parsing email, phone, url