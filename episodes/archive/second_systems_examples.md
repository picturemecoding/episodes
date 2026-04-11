# second_systems_examples

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

We have two questions:

- What makes second systems so hard?
- Are second systems inevitable?

Brooks’s on “Second System Effect”

*An architect’s first work is apt to be spare and clean. He knows he doesn’t know what he’s doing, so he does it carefully and with great restraint.*

*As he designs the first work, frill after frill and embellishment after embellishment occur to him. These get stored away to be used “next time.” Sooner or later the first system is finished, and the architect, with firm confidence and a demonstrated mastery of that class of systems, is ready to build a second system.*

*This second is the most dangerous system a man ever designs. When he does his third and later ones, his prior experiences will confirm each other as to the general characteristics of such systems, and their differences will identify those parts of his experience that are particular and not generalizable.*

*The general tendency is to over-design the second system, using all the ideas and frills that were cautiously sidetracked on the first one. The result, as Ovid says, is a "big pile.*

IRS Master File System

Coded in assembly language in 1961, the IMF seems small, at 200,000 lines of code. But the nature of assembler is that each line does a lot. Somehow replicating the logic in a more contemporary language has resisted efforts dating back a couple of decades. That in turn has delayed many service improvements and operational efficiencies the IRS has wanted to make.

<https://federalnewsnetwork.com/tom-temin-commentary/2020/10/irs-details-strategy-for-replacing-its-most-ancient-computer-code/>

Joel Spolsky [called rewriting code from scratch](https://www.joelonsoftware.com/2000/04/06/things-you-should-never-do-part-i/) the "single worst strategic mistake that any software company can make." (Dropbox piece)

From the above Spolsky article…

We’re programmers. Programmers are, in their hearts, architects, and the first thing they want to do when they get to a site is to bulldoze the place flat and build something grand. We’re not excited by incremental renovation: tinkering, improving, planting flower beds.

There’s a subtle reason that programmers always want to throw away the code and start over. The reason is that they think the old code is a mess. And here is the interesting observation: they are probably wrong. The reason that they think the old code is a mess is because of a cardinal, fundamental law of programming:

It’s harder to read code than to write it.

Second System Case Studies

- AWS switches off [final Oracle database](https://aws.amazon.com/blogs/aws/migration-complete-amazons-consumer-business-just-turned-off-its-final-oracle-database/)
  + “We migrated 75 petabytes of internal data stored in nearly 7,500 Oracle databases to multiple AWS database services.” and “More than 100 teams in Amazon’s Consumer business participated in the migration effort.”
- [IRS Individual Master File](https://federalnewsnetwork.com/tom-temin-commentary/2020/10/irs-details-strategy-for-replacing-its-most-ancient-computer-code/)
- Dropbox [rewrites their “sync” system](https://dropbox.tech/infrastructure/rewriting-the-heart-of-our-sync-engine). This is a solid piece that covers the tradeoffs.
- Mozilla inventing Rust and then [rewriting their browser in it](https://hub.packtpub.com/mozilla-engineer-shares-the-implications-of-rewriting-browser-internals-in-rust/) so that they could have concurrency with memory safety
- Discord [switch from Go to Rust](https://discord.com/blog/why-discord-is-switching-from-go-to-rust): garbage collection pauses and overall latencies
- Twitter’s move away from RoR: <https://www.infoq.com/news/2012/11/twitter-ruby-to-java/>

What Makes People Decide to Throw Away a Working System and Build a Whole New Thing

- Dropbox: very hard to ship and inconsistencies in production. Hard to add contributors. System was slow. Hard to test.
- AWS Oracle: too much work and too high costs managing these Oracle DBs
- IRS IMF: Nobody can maintain the thing. They can’t build modern stuff (like APIs) on top of it.
- Mozilla: too many bugs (concurrency, memory safety, memory leaks)
- Discord: system was slow

Looking for patterns in the above:

- System is too slow
- It’s too hard to add contributors (nobody can maintain what we have)
- Costs too much
- Too buggy
- Can’t take advantage of modern features
- The system is too large
- My gut feeling is that rewrites are more often caused by architecture problems rather than code problems. A well-organized system that’s just slow or buggy can probably still be profiled and optimized, but *architecture* is hard to change incrementally.
- Things that seem to have led to do-overs:
  + Different hardware
    - Adapt to massively parallel or vectorized processors, GPUs
  + New distribution models
    - Web apps/SaaS vs desktop
    - Mobile
    - Cloud in general vs. on-prem
  + Ports to new languages

Software is meant to be deployed on computer hardware. Is it connected to an *era* of computer hardware? In other words, if you advance a decade in computer hardware is the software unlikely to stay the same?

*I think the answer is, probably, “yes” with some conditions. For example, a lot of software survived for more than a decade because it was generally backward compatible with the Intel instruction set, so even though the computer’s hardware changed a fair bit, the software would still run. I think there was a point even there though where 32-bit stuff had to be ported to 64-bit hardware.*

What Makes Replacing Systems Hard

- Potentially unknown inputs and outputs (who talks to this thing and how do they talk to it in all cases)
- Unanticipated/unknown behaviors

There's an irony here: the things that contribute to people wanting to replace services also make it significantly harder to do so.

What Would It Mean If Second Systems Were Inevitable or Not

- Not: you can build the right thing from the start and never move on from it
- Inevitable: You could potentially *know in advance* that you are moving on someday and build with that in mind?
- Building a second system is like waving a white flag in the face of the first system: it can no longer be maintained. We give up. Does this mean we will always lose against our successful systems?

Both of the above require guessing about the future (premonitions).

### An Anecdote for fun…

In 2018 I was at Kubecon going up the elevator to the top of the space needle. I’d waited in a line that snaked around the outside of the base of the Space Needle and then wound inside the glass ground-floor building and the gift shop up to the elevator doors. We’d gotten this privilege for free as members of the Kubecon 2018 conference and while I was in line I chatted a bit with these two guys who were behind me and who were also on the elevator to the top with me afterward. Have you ever been up to the top of the Space Needle?

The guys behind me in line for the Space Needle elevator were Oracle sales people and I’d gathered that they’d been sneered at everywhere they went at Kubecon. I could only imagine people at Kubecon derisively wondering why anyone would pay the famously exorbitant fees to run Oracle databases? Anyway, these guys were friendly enough but they seemed a bit tired and beleaguered. They did also complain about the food and the lines and things: they almost seemed like people who ate steak and rode in private cars everywhere…

Anyway, in the elevator up to the top of Space Needle someone said, “Where’s Jeff Bezos’ house?” and I said, “these guys probably know!” and then one of the Oracle sales guys dourly chided me, “Don’t you read the tech news? He’s not a customer of ours anymore.”

I hadn’t heard this and I was kind of overcome to imagine what it would be like to work for Amazon and be told “we’re going to stop using Oracle DBs and transition everything over.” What an unimaginable mountain of work!

Anyway, that guy was right: Werner Vogels wrote this blogpost around that time about how they had turned off their first huge Oracle DB and within a few years they were planning to turn off all of the things they had running!

This is what I would call a “second system” problem. They’re hard, expensive to migrate to. I wouldn’t characterize *all* second systems as *bad*, as long as we’re aware of the risks that Fred Brooks pointed out.