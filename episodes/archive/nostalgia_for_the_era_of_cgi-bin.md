# Nostalgia for the Era of cgi-bin

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

Erik Music: Manu Chao, *Viva Tu*

Mike Music: Blood Incantation, *Absolute Elsewhere*

*“the soundtrack to a Herzog-style Sci-Fi epic about the history of/battle for human consciousness itself, via a 70s Prog album played by a 90s Death Metal band from the future.”*

This is an episode about the pre-web-2.0 internet days: bulletin boards! Cgi-bin! Webrings! Before “google” became a verb (it is apparently [fading as a verb](https://www.businessinsider.com/google-losing-status-as-verb-genz-2024-9#:~:text=Gen%20Z%20prefers%20'searching'%20over,evolving%20tech%20and%20user%20behavior.)…)

Sort of inspired by the removal of the [cgi](https://docs.python.org/3/library/cgi.html) module from Python 3.13 (deprecated in 3.11). It’s hard now to imagine the pre-AJAX era of the internet. In the mid 90s the places where I worked were using just CGI-Bin, no Javascript at all. I knew people writing server-side code in C! There was still enthusiasm for the idea of Java “applets”. If you were a real web hotshot, you might know ColdFusion. GeoCities was a kind of ur-MySpace. AOL was still sending out CD ROMs (this didn’t stop until 2006!!)

I had an Angelfire website. Fortunately, I don’t remember the URL.

## How Did the Web Used to Work?

Speaking of webservers, Apache came out in 1995.

- Cgi-bin: early internet… Bulletin boards! Mike’s image rectangles of text story.
- Java applets
  + I worked for a company that tried to replace its entire suite of desktop software with browser-based applets. Fortunately, everyone realized it was bonkers before the company burned itself to the ground.

cgi-bin: “Common Gateway Interface” and bin is just the “bin” (binary) we know and love

This was just a bucket of scripts that the webserver would run based on url path (the path means a file to look up). They would read stdin, write stdout (HTML). They had access to environment variables.

### Mining the Wayback Machine for Nostalgia

- Should we talk about the beauty of webarchive? <https://web.archive.org/>
- Netscape Navigator
- Early days of Google where it always included a message that read “this query took ….seconds”. They had the button “I’m feeling lucky” which would take you immediately to the first result of a search.
- Alternate search engines: dogpatch? Alta vista. Yahoo.
- Napster
- Dan’s Gallery of the Grotesque: a gore site which if memory serves was laid out like a house with “rooms”. You would click on a room to go to that webpage and see how all the horrific images inside (Kurt Cobain’s suicide). The site had a whole exegesis on why it existed and I’m not too clear on it but I vaguely remember it arguing that the internet allows access to information in ways we had yet to consider (at that time).

## The Meaning and History of the Web Bulletin-Board

- These still kind of exist
- Examples of some that left an impression on us:
  + [Dreamless.org](https://en.wikipedia.org/wiki/Joshua_Davis_%28designer%29#Dreamless) (Josh Davis, the site looked like something out of neuromancer, Davis called parties “riots”).
  + Debian forum
- Myspace and Facebook threatened to kill this. They never provided the same experience, though. Facebook is a walled-garden: they don’t want you to ever leave.

Dreamless and threadless are the *potential for a community*. Example: if you’ve ever used Threadless to order t-shirts, that started as a t-shirt design competition on dreamless. History of Threadless ([profile of Jake Nickell from 2011](https://chicagoreader.com/arts-culture/jake-nickell-the-entrepreneur/) in the Chicago Reader):

I [Jake Nickell] was on this forum called Dreamless.org. It was this group of artists all around the world. It was an invite-only forum, so it was these really talented artists. They held this event every year called New Media Underground Festival where they’d all hang out together and share their thoughts. They held a T-shirt design challenge on the forum. I entered into it and I won.

Literally an hour later, after learning that I won, I started Threadless by starting a thread on that forum saying, “Post up designs and I’ll print the best ones, make them into T-shirts that we can all have.” That was 2000. It was in the age of the forum that I started Threadless. It worked out pretty well. Even though it’s different now, I think that the underlying idea of connecting with people online, just as a communications platform, is still the same. It’s just different tools.

It wasn’t really a business at first. He started by just dumping all the money into shirts. Because there were users in the UK, he would ship internationally, which was somewhat uncommon for the web at that time. He never questioned it:

That was something I never second-guessed because there are people on the forum who live in the UK; I want to be able to ship them the shirts. A lot of e-commerce companies wouldn’t do international in the beginning because it’s too hard or something, but for me it was just a given, because that’s where the people are.

This is the kind of spirit and possibility I remember from the early days of the internet: *stuff was happening*. Small communities of people were *making things possible*.

## Comments on Nostalgia

Nostalgia is dangerous because it’s never accurate. Heidegger quotes the German poet Novalis: "Philosophy is really homesickness: the urge to be at home everywhere."

I have relatively little nostalgia for this era, except that, maybe, i wish that simple request/response designs were still popular, and i wish that people had actually leveraged the hypertext features of the original WWW design. Other than, say, Wikipedia that style of website seems rare now.

### Downsides (including security)

- Ashley Madison doc (they started in 2001). SQL injection! Fast and loose with security.
- Java applets… (servlets?)
- Flash.
- Basically no good way to do interactive applications.
- How did we scale things back then? (More Apache servers: trees of them?)

### Good Things

- Search engines *actually searched for stuff*
- No pop-ups (although remember when they started and before browsers had pop-up blockers baked in? That sucked)
- No cookie settings!
- More emphasis on user-generated content, not just social media
- Simplicity (i wrote a couple of articles for Linux Journal in the 90s on how to write web servers in Perl)
- Because of simplicity, the tools were readily available: anyone could do it!
- Kibo

## Overall Thesis

- Disillusionment: open internet before ads and scams!
- It’s so sad the only internet business models are ads, scams, and what…
- There was so much promise and possibility at the beginning: it felt like things were rapidly expanding outward and you couldn’t conceive of all the possibilities.
- I started reading articles on the “death of the Internet” in, probably, ‘92? [What was the argument at the time?]

## Cory Doctorow’s Enshittification

In one of the early paragraphs of his essay, Doctorow cites the original paper written by the creators of Google, where they note that search engines eventually become antagonistic to their users’ interests so they can please the advertisers. The interests of advertisers and users are are at odds, in other words.

[Original blogpost](https://www.wired.com/story/tiktok-platforms-cory-doctorow/) in Wired

This shell-game with surpluses is what happened to Facebook. First, Facebook was good to you: It showed you the things the people you loved and cared about had to say. This created a kind of mutual hostage-taking: Once a critical mass of people you cared about were on Facebook, it became effectively impossible to leave, because you'd have to convince all of them to leave too, and agree on where to go.

Then, it started to cram your feed full of posts from accounts you didn't follow. At first, it was media companies, whom Facebook preferentially crammed down its users' throats so that they would click on articles and send traffic to newspapers, magazines, and blogs. Then, once those publications were dependent on Facebook for their traffic, it dialed down their traffic. First, it choked off traffic to publications that used Facebook to run excerpts with links to their own sites, as a way of driving publications into supplying full-text feeds inside Facebook's walled garden.

This made publications truly dependent on Facebook—their readers no longer visited the publications' websites, they just tuned into them on Facebook. The publications were hostage to those readers, who were hostage to each other. Facebook stopped showing readers the articles publications ran, tuning The Algorithm to suppress posts from publications unless they paid to "boost" their articles to the readers who had explicitly subscribed to them and asked Facebook to put them in their feeds.

Now, Facebook started to cram more ads into the feed, mixing payola from people you wanted to hear from with payola from strangers who wanted to commandeer your eyeballs. It gave those advertisers a great deal, charging a pittance to target their ads based on the dossiers of non-consensually harvested personal data they'd stolen from you.

Sellers became dependent on Facebook, too, unable to carry on business without access to those targeted pitches. That was Facebook's cue to jack up ad prices, stop worrying so much about ad fraud, and to collude with Google to rig the ad market through an illegal program called Jedi Blue.

Today, Facebook is terminally enshittified, a terrible place to be whether you're a user, a media company, or an advertiser. It's a company that deliberately demolished a huge fraction of the publishers it relied on, defrauding them into a "pivot to video" based on false claims of the popularity of video among Facebook users. Companies threw billions into the pivot, but the viewers never materialized, and media outlets folded in droves.

Doctorow criticizing the term “monetize”:

"Monetize" is a terrible word that tacitly admits that there is no such thing as an "attention economy." You can't use attention as a medium of exchange. You can't use it as a store of value. You can't use it as a unit of account. Attention is like cryptocurrency: a worthless token that is only valuable to the extent that you can trick or coerce someone into parting with "fiat" currency in exchange for it. You have to "monetize" it—that is, you have to exchange the fake money for real money.

Doctorow’s claim:

In the beginning, there were Bellheads and Netheads. The Bellheads worked for big telcos, and they believed that all the value of the network rightly belonged to the carrier. If someone invented a new feature—say, Caller ID—it should only be rolled out in a way that allows the carrier to charge you every month for its use. This is Software-As-a-Service, Ma Bell style.

The Netheads, by contrast, believed that value should move to the edges of the network—spread out, pluralized. In theory, Compuserve could have "monetized" its own version of Caller ID by making you pay $2.99 extra to see the "From:" line on email before you opened the message— charging you to know who was speaking before you started listening—but they didn't.

The Netheads wanted to build diverse networks with lots of offers, lots of competition, and easy, low-cost switching between competitors (thanks to interoperability).

…

The Netheads were right: Technological self-determination is at odds with the natural imperatives of tech businesses. They make more money when they take away our freedom—our freedom to speak, to leave, to connect.