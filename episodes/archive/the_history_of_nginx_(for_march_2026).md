# A History of Nginx

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

Question: did you ever hear the one about how nginx was created to serve Russian porn sites?

The idea being that porn sites in Russia had much higher traffic than other contemporaneous websites so it necessitated the creation of a super fast, highly scalable web server.

Motivation: At work, I did a large PoC to build a Gateway API implementation and plan out moving to it off the nginx-ingress-controller. We have to do this because in march 2026, nginx-ingress-controller is going away. Here’s a post on this from november 2025, from official kubernetes blog:

To prioritize the safety and security of the ecosystem, Kubernetes SIG Network and the Security Response Committee are announcing the upcoming retirement of Ingress NGINX…. In March 2026, Ingress NGINX maintenance will be halted, and the project will be retired. After that time, there will be no further releases, no bugfixes, and no updates to resolve any security vulnerabilities that may be discovered. The GitHub repositories will be made read-only and left available for reference.

Anyway, this question popped into my head: *was* nginx actually created to serve Russian porn sites? As I started researching this question, I realized that we should do a picture-me-coding mythbusters episode and talk about the history of this project.

### Background

Imagine it’s 1999, internet popularity is growing and we’ve got an interesting technical problem on our hands: it’s hard to find software that can run on servers allowing a whole lot of simultaneous network connections. A guy named Dan Kegel called this the *C10K* problem, which stands for 10,000 concurrent connections. Dan wrote this influential post on Linux Weekly News, saying “hey, computer hardware is cheap enough and the linux operating system is viable to be able to do this.” His argument is: this should be doable and we are going to have to solve this problem.

Here’s how his article started:

It's time for web servers to handle ten thousand clients simultaneously, don't you think? After all, the web is a big place now.

And computers are big, too. You can buy a 1000MHz machine with 2 gigabytes of RAM and an 1000Mbit/sec Ethernet card for $1200 or so. Let's see - at 20000 clients, that's 50KHz, 100Kbytes, and 50Kbits/sec per client. It shouldn't take any more horsepower than that to take four kilobytes from the disk and send them to the network once a second for each of twenty thousand clients. (That works out to $0.08 per client, by the way. Those $100/client licensing fees some operating systems charge are starting to look a little heavy!) So hardware is no longer the bottleneck.

In 1999 one of the busiest ftp sites, cdrom.com, actually handled 10000 clients simultaneously through a Gigabit Ethernet pipe. As of 2001, that same speed is now being offered by several ISPs, who expect it to become increasingly popular with large business customers.

[E] I didn’t work in software at the time, but I remember people talking intensely about the C10K problem: it was seen as a scaling limit of the internet. You could buy more machines, of course, and you could increase throughput, but how were you going to get the whole world connected to some popular websites *simultaneously*?

This problem seems quaint now but it’s got such a rad and memorable name: I think that’s why it captured the zeitgeist: c10k. It looks like a drug in a william gibson novel or something. It reads like a radical faction in sci-fi movie. Imagine telling someone, “I cracked the c10k problem.” It sounds *historic*. Whoever heard this would shower you with praise!

### Technical underpinnings

As a software problem, starting from scratch, how would you try to solve this problem? There's a limited number of processes, threads, file descriptors, socket connections… resources are finite…

[Mike]

Sockets are, in the spirit of Unix, also file descriptors. So if you had an application that was just listening on one socket you could just issue a read and block until data arrived. That wasn’t at all uncommon back in the 1980s, in the pre-web era.

To do a web server you need to multiplex on sockets. Back in the day the idea of multiplexing sockets was fairly limited. The idea of 10k connections would have seemed insane. In my Unix Network Programming book, the only multiplexing option discussed is select(), one of the weirdest system calls ever. The book discusses it in the context of a print server, since httpd was still a glimmer in the eye of TBL. With select you could specify a set of sockets, and it would block up to some specified timeout or until data arrived on one of them. Select had a fixed number of file descriptors (usually 1024) and it had to scan through them. Not long afterward, Unix introduced poll(), which was way more sensible, but still had to scan through all N sockets.

### Igor Sysoev, the author of Nginx

So there’s a guy from Russia, working in tech companies in the 90s and early 2000s named Igor Sysoev and he created nginx.

Now, one interesting thing about Sysoev is that he was a hobbyist; he was *not* hired as a software developer originally, but a sysadmin. He did software dev in his spare time so he could *automate his job*. This was apparently the thing about working at this company [Rambler](https://en.wikipedia.org/wiki/Rambler_%28portal%29), a massively popular web portal and search engine (think Yahoo! of Russia) they hired top-talent, but in order to attract these people, they told them, “you all get to work on your own hobby projects while you are here”.

[He says] “In addition to the direct work of the system administrator, I started writing programs in my free time. It should be noted that programming was not part of my job responsibilities. ”

The fact that Sysoev has his own project was known to Rambler. “When hiring Sysoyev - I hired him in 2000 - it was specifically agreed that he has his own project, and he has the right to do it. It was then called something like mod\_accel, ”says Igor Ashmanov, the former executive director of Rambler.

“Sysoev worked on his project on his own, without instructions from management, in his spare time from the main work. Moreover, he was not, as far as I know, a software developer, so this project could not be entrusted to him in any way, ”says Denis Kalinin, the former general director of Rambler. [[link](https://rusecrets.com/articles/how_rambler_pulls_igor_sysoevs_hobby)]

…

In the early 2000s, staff shortages in the industry were so great that Rambler, in order to hire stars, had to agree that many had their own pet projects, Sokolova explains. Denis Kalinin agrees with her: “Rambler hired the best of the best, and the stratum of people with good experience is still thin enough, and then it was thousands of times more complicated. The Internet was small, and everyone who came to us, most likely, did some kind of their own project. For example, when I started working at the company, I continued developing software for space equipment for one of the physics institutes. Some continued to develop FreeBSD, others - Postgres. ”

Oleg Bartunov says the same thing: “It was taken for granted: everyone who worked then [at Rambler] had their own interests and could deal with them in their free time ... And we must understand that in general the whole Rambler was built on open source technology. We were very proud of this, everyone tried to give something there. ” [same link]

From: ​​<https://en.wikipedia.org/wiki/Igor_Sysoev#Nginx>:

The development of Nginx started in 2002 after Igor had been administering the Rambler servers, which had been running Apache, for two years, but those lacked scaling capability beyond 1000 users on a single server.

So Igor has his hobby project, which eventually becomes Nginx and one of his early goals is to crack the notorious C10K problem. [In his own words](https://web.archive.org/web/20131019145106/http%3A//www.freesoftwaremagazine.com/articles/interview_igor_sysoev_author_apaches_competitor_nginx):

Back then, I was trying to overcome certain barriers of scaling the web infrastructure of a large online media company I worked for. In particular, the difficulties of handling many concurrent connections, reducing latency and offloading static content, SSL and persistent connections were my main interest. There weren't any reliable production quality web server software to crack so-called C10K problem (handling of at least 10,000 of concurrent connections, outlined by Dan Kegel). So in a sense I decided to solve both practical and "academic" problems.

How do you do this, technically? The wikipedia page on his work gives a hint:

Increasing the number of servers could help, but initiated the problems of its own, and Igor conceived an idea to do scaling inside a server, increasing the number of possible connections. At first the idea was driven by mere technical curiosity, until after about two years he was offered to try it on Zvuki.ru web library of music. [[wiki](https://en.wikipedia.org/wiki/Igor_Sysoev#Nginx)]

For a better description of the technical underpinnings, Cloudflare has a [pretty cool breakdown](https://blog.cloudflare.com/how-we-scaled-nginx-and-saved-the-world-54-years-every-day/) of a contribution they made more recently to scale it up. But check out this opening on nginx:

NGINX is one of the programs that popularized using event loopsto solve the C10K problem. Every time a network event comes in (a new connection, a request, or a notification that we can send more data, etc.) NGINX wakes up, handles the event, and then goes back to do whatever it needs to do (which may be handling other events). When an event arrives, data associated with the event is already ready, which allows NGINX to efficiently handle many requests simultaneously without waiting.

### Apache

What was Nginx replacing? Initially released in 1995, Apache was *the web server of choice* for a long time. From the Apache project’s [history page](https://httpd.apache.org/ABOUT_APACHE.html):

In February of 1995, the most popular server software on the Web was the public domain HTTP daemon developed by Rob McCool (sidebar: he was an undergraduate when he wrote httpd…) at the National Center for Supercomputing Applications, University of Illinois, Urbana-Champaign. However, development of that httpd had stalled after Rob left NCSA in mid-1994, and many webmasters had developed their own extensions and bug fixes that were in need of a common distribution. A small group of these webmasters, contacted via private e-mail, gathered together for the purpose of coordinating their changes (in the form of "patches"). Brian Behlendorf and Cliff Skolnick put together a mailing list, shared information space, and logins for the core developers on a machine in the California Bay Area, with bandwidth donated by HotWired.

CGI and the module system made it super easy to extend Apache with all kinds of functionality.

Apache was itself replacing NCSA-httpd which used a preforked process for each new connection (connection -> fork). Apache used this same design, essentially one process per connection. Apache didn’t use a thread per connection until MPM which was a complete redesign in Apache 2.0, but even the thread-per-connection style is resource-intensive compared to event-driven. [[architecture](https://www.fmc-modeling.org/category/projects/apache/amp/3_1Overview.html)]

IIRC, the way the Apache originally worked (which i think was also true of httpd) was that it would open a bunch of sockets, poll on them, and then fork() to handle the actual request. At first that was fine, but fork() is fairly slow. There’s still a “prefork” mode of Apache that forks some number of child processes to handle requests. At some point they started using threads, but that’s still a little slow and uses a fair bit of memory. In addition, threads have to be scheduled by the kernel. The current MPM version of Apache does have an “event” module, but i don’t think it works like nginx (i think it still maintains a threadpool?)

The innovation that changed things and solved the c10k problem was *epoll* (in Linux) and *kqueue* (BSD). I’ve never really used these things in anger, but my understanding is that both essentially allow that kernel to signal other processes when data arrives on socket. That enabled the nginx way of doing things with an event loop.

The new hotness is *io\_uring*, but that doesn’t seem to have penetrated the mainstream web server market too much yet.

### What Does Free Mean

There’s an interesting note on that Apache history page in answer to the question “Why Apache Software is Free”:

Apache Software exists to provide robust and commercial-grade reference implementations of many types of software. It must remain a platform upon which individuals and institutions can build reliable systems, both for experimental purposes and for mission-critical purposes…

We believe that the tools of online publishing should be in the hands of everyone, and that software companies should make their money by providing value-added services such as specialized modules and support, amongst other things…

To the extent that the protocols of the World Wide Web remain "unowned" by a single company, the Web will remain a level playing field for companies large and small. Thus, "ownership" of the protocols must be prevented. To this end, the existence of robust reference implementations of various protocols and application programming interfaces, available free to all companies and individuals, is a tremendously good thing.

Furthermore, the Apache Software Foundation is an organic entity; those who benefit from this software by using it, often contribute back to it by providing feature enhancements, bug fixes, and support for others in public lists and newsgroups. The effort expended by any particular individual is usually fairly light, but the resulting product is made very strong. These kinds of communities can only happen with freely available software -- when someone has paid for software, they usually aren't willing to fix its bugs for free.

When Nginx popularity started to take off around 2011, 2012, there were concerns about whether it was “free” or a lure for some commercial offerings. The interview with Igor Sysoev touches on this:

TM: Are you concerned about alienating some adopters, who might end up seeing Apache as the "really free" option and NGINX as the expensive one?

For the people who are worried about whether NGINX is "really free" I can point to the list of changes that were introduced to the open source version in just the past few months since the company has started (there are literally dozens of improvements and bugfixes).

I believe this is the best illustration in regards to how much we're devoted to maintaining the BSD-licensed software. In fact, we're seeing a growing number of new users interested in NGINX and we're quite happy the news about the company and the funding didn't alienate them. We all believe that the fact the company has started is really beneficial for the project as it greatly secures and streamlines the development.

### Software Horse Races

About 10 years ago, I worked for a pretty large tech company and they had an internal portal for managing Apache instances: you didn’t have to write the apache confs by hand, so of course I was that midlevel engineer who made snarky, dumb comments about how “we should probably be using nginx,” and loudly associated the big, lumbering tech corp with slow, old software. That was pretty dumb of me! But I was taken in then by the software horse races: nginx -> cool and fast. Apache -> old and slow. Would you rather be cool and fast or old and slow, Mike?

Nginx is pretty popular now! From the wikipedia page on [Apache](https://en.wikipedia.org/wiki/Apache_HTTP_Server):

As of March 2025, Netcraft estimated that Apache served 17.83% of the million busiest websites, with the other top four being Cloudflare at 22.99%, Nginx at 20.11%, and Microsoft Internet Information Services at 4.16%. According to W3Techs' review of all web sites, in April 2025 Apache was ranked second at 26.4% and Nginx first at 33.8%, with Cloudflare Server third at 23.5%

The early growth of the web was made possible by Apache and that growth was accelerated by Nginx, a healthy competition.

So there’s this interesting phenomenon here: web server development propelled web traffic growth, it made it possible, and fed back in requiring the servers themselves to grow more performant. Further, the open-software part of the two most popular projects contributed because it wasn’t *only* large companies who could afford to deploy this software.

Back to our myth Russian porn sites were the raison d’etre?

Among the first customers were Zvuki.ru, Estonia dating service Rate.ee and Russian dating service Mamba.ru. There were zero promotion efforts, as Igor didn't have the ability or desire to do that in addition.

BUSTED

## References

​​[Igor Sysoev - Wikipedia](https://en.wikipedia.org/wiki/Igor_Sysoev#Nginx) (This wikipedia article is pretty good.)

Interview with him from 2012: [https://web.archive.org/web/20131019145106/http://www.freesoftwaremagazine.com/articles/interview\_igor\_sysoev\_author\_apaches\_competitor\_nginx](https://web.archive.org/web/20131019145106/http%3A//www.freesoftwaremagazine.com/articles/interview_igor_sysoev_author_apaches_competitor_nginx)

Apache history: <https://httpd.apache.org/ABOUT_APACHE.html>

<https://rusecrets.com/articles/how_rambler_pulls_igor_sysoevs_hobby>

https://kubernetes.io/blog/2025/11/11/ingress-nginx-retirement/