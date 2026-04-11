# news_roundup_nov_22_2025

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025

---

## Episode Summary

---

## Music

Erik: Loads of new music. I’m hearing these things a lot -> Snocaps: Katie Crutchfield, better known as Waxahatchee, her sister Allison Crutchfield, MJ Lenderman and Brad Cook. ~~Also Aesop Rock, I Heard It’s a Mess There Too. Still funny, still razor sharp. He has a song called “The Cut” on here I really like.~~

Mike: Rosalía - Lux. Basically a pop album, but with elements of orchestral and choral music. Supposedly in 13 languages, though I’ve only identified about 6.

## What’s With the News?

- New Yorker article on data centers and AI
- Korean data center
- AWS outage
- Cloudflare outage
- New Yorker article on “is AI thinking?”

## New Yorker AI Data Centers Article

This piece is titled “Information Overload: Inside the Data Centers that Train AI and Drain the Electrical Grid”

From New Yorker - [link](https://www.newyorker.com/magazine/2025/11/03/inside-the-data-centers-that-train-ai-and-drain-the-electrical-grid) October 27, 2025 (Published in the November 3 issue)

Written by Stephen Witt, who gained access to a Microsoft data center and a CoreWeave data center, he writes about how these data centers are operating and what they’re doing. He starts with:

The leading independent operator of A.I. data centers in the United States is CoreWeave, which was founded eight years ago as a casual experiment.

They were originally traders at a hedge fund in New York and they started mining cryptocurrency in order to use it as their entry for their fantasy football league. They made so much money that they wanted to scale it up, so they bought several thousand GPUs and built a data center in the garage of one of the founders.

So now these guys have millions of GPUs in a whole bunch of data centers. The article goes on to say that after Microsoft partnered with ChatGPT, they couldn’t keep up with their computing needs, so they went to CoreWeave to fill in the gaps.

Inside the CoreWeave data center, he says someone opens a rack to show him and (wearing earplugs), he writes: “The noise was unholy, as if I’d opened a broom closet and found a jet inside. I watched the blinking lights and the spinning of the fans. “Tinnitus is an occupational hazard,” Conley [a CoreWeave exec] shouted at me.”

The writer then talks about how customers will use CoreWeave’s data centers to train these massive models. Sometimes one of their customers will use the entire data center for weeks at a time, which they call a “hero run.”

“A weeks long hero run can use tens of thousands of GPUs and require ten trillion operations, which is more than the observable stars in the universe.”

The writer visited a Microsoft data center where he was sworn to secrecy on its location.

“After donning a pair of steel-toed boots and watching a PowerPoint presentation, I passed through a security checkpoint and into the inner sanctum. The facility was quieter, tidier, and more spacious than the CoreWeave center.”

*SHADE*

“…Across all five sheds, the area dedicated to computing was the equivalent of twenty football fields.”

This article delves into matrix multiplication as a side note, after quoting the mathematician Hardy saying “Beauty is the first test: there is no permanent place in the world for ugly mathematics,” saying that matrix multiplication is like a man hammering nails into a board. So the implication is that these data centers are scaling up thousands of big dumb hammers.

Is it almost kind of lame to imagine twenty football fields worth of computing running 24 hours a day to train a bunch of weights in a neural network? Doesn’t it seem like, “Wait, this is the current pinnacle of our civilization?” Imagine a group of aliens shows up and sees all the fans and blinking lights: what would they imagine? I saw this rad post of three-colored kittens the other day…

People are always talking about power and water usage and I think this piece was an attempt to capture some of that. He wanders down the road to talk to a local farmer who says that his neighbors are selling land to make way for data centers. He also tells him, “we use more water than they do.” But what about the electricity required to run all of this?

When a data center comes online, retail customers usually help to foot the bill: American utilities sought almost 30 billion dollars in retail rate increases in the first half of 2025. This spring utilities requested almost double the rate hikes from the same period a year earlier. An analysis by Bloomberg estimated that, in areas near data centers, wholesale electricity costs have risen by more than two hundred per cent in the past five years.

Data centers must operate twenty-four hours a day to be economically viable. In the near term, new data centers will largely be powered by fossil fuels.

The article talks a bit about the speculative bubble around AI in relation to building data centers and the writer offers this comparison:

In the 19th century, the building of the railroads contributed an estimated 6 percent (to US GDP). Railroads transformed America and generated tremendous – if unevenly distributed – prosperity, but the frenzy also produced one of the largest speculative bubbles in history.

### Other Data Center/AI Bubble Stories

- Korean data center designed and built by AI
  + <https://www.wsj.com/tech/a-big-data-center-planned-in-south-korea-could-be-built-and-run-by-ai-2bdf26e5?st=18FHuS&reflink=desktopwebshare_permalink>
  + VoltAI - Slightly scary company trying to give AI access to the physical world
- Crazy amount of debt accumulated by Oracle et. al to build data centers for OpenAI
  + <https://finance.yahoo.com/news/hedge-against-ai-crash-emerges-181723667.html>

## Outages Galore

Last month there was a massive AWS outage. Here’s a description of the impact from an article on [Cnet](https://www.cnet.com/tech/services-and-software/amazon-web-services-outage-october-20-2025/):

The outage rendered huge portions of the internet unavailable for much of the workday for many people. As Monday rolled along, it affected more than 2,000 companies and services, including Reddit, Ring, Snapchat, Fortnite, Roblox, the PlayStation Network, Venmo, Amazon itself, critical services such as online banking and household amenities such as luxury smart beds.

<https://www.cnet.com/tech/services-and-software/amazon-web-services-outage-october-20-2025/>

My son uses an education app for school called Canvas and it went down and all of his friends were like, “yay!”

Also:

Downdetector saw over 9.8 million reports, with 2.7 million coming from the US, over 1.1 million from the UK, and the rest largely spread across Australia, Japan, the Netherlands, Germany and France.

Turns out that the outage was a result of a distributed systems failure and there’s a even a nice TLA+ model of the outage, provided by Murat: <https://muratbuffalo.blogspot.com/2025/11/tla-modeling-of-aws-outage-dns-race.html>

There was a race condition in the DNS planner for DynamoDB, where the goal was to plan DNS records and then enact them (as separate processes). There is a race condition between systems checking if the plans are active, or stale and ready to be deleted where a system that shows up first but moves more slowly can end up enacting a plan that’s immediately stale.

Distributed systems strikes again!

### Cloudflare

The issue was not caused, directly or indirectly, by a cyber attack or malicious activity of any kind. Instead, it was triggered by a change to one of our database systems' permissions which caused the database to output multiple entries into a “feature file” used by our Bot Management system. That feature file, in turn, doubled in size. The larger-than-expected feature file was then propagated to all the machines that make up our network.

The software running on these machines to route traffic across our network reads this feature file to keep our Bot Management system up to date with ever changing threats. The software had a limit on the size of the feature file that was below its doubled size. That caused the software to fail.

<https://blog.cloudflare.com/18-november-2025-outage/>

See here for Rust-related part: <https://blog.cloudflare.com/18-november-2025-outage/>

Omg they used unwrap… This is what all of those social media rust people were talking about about allowing unwrap to pass code review!

### Good Rust News

The Cloudflare outage has been blamed on Rust, so here’s a potential counter

[Rust Adoption Drives Android Memory Safety Bugs Below 20% for First Time](https://thehackernews.com/2025/11/rust-adoption-drives-android-memory.html)

### Worth a Mention: “Is AI Thinking?” Article

This piece is called “Open Mind: The case that AI is thinking”, published [in the New Yorker](https://www.newyorker.com/magazine/2025/11/10/the-case-that-ai-is-thinking) on November 10th. It was written by James Somers, who is a software developer who’s done a lot of web stuff, including Python and Javascript (he’s one of us; I should send him our podcast…).

There’s a lot of cool theory about *thinking* in this piece, using insights from neuroscientists and others who have started studied AI in order to use it as a model for thinking. Choice quote: “Al enables scientists to place thinking itself in a wind tunnel.”

This whole piece is worth reading, but I just wanted to highlight two aspects of it:

1. The writer talks about a Finish-American Cognitive Scientist named Pentti Kanerva who “noticed some unusual properties in the mathematics of high-dimensional spaces… where any two random points may be extremely far apart, but counterintuitively each point also has a large cloud of neighbors around it, so you can easily find your way to it if you get ‘close enough’.” And all of this reminded this researcher Kanerva of how *memory* works. And then a few paragraphs later, the article cites a researcher at Anthropic who says that Transformer models (like chatGPT), the mathematics in there approximates the model proposed by Kannerva in the 80s.
2. The idea of compression appears in here: that memories are a form of compressed data in the brain and further that language is compressible in interesting ways (which is why training LLMs on language has been really successful at producing what looks like thought, but you can’t train them on video and have them make suggestions that follow normal physics or the patterns of our world, like how buildings are laid out).

The other thing this article talks about with regard to thinking is that our brains are constantly updating themselves, but neural networks after training *are not*: they’re frozen in space. When you want them to produce different text inspired by something, you give them examples which they store and reread alongside any new prompt that comes in.

### Misc

“Adversarial Poetry as a Universal Single-Turn Jailbreak Mechanism in Large Language Models”

We present evidence that adversarial poetry functions as a universal single-turn jailbreak technique for Large Language Models (LLMs)... Poetic framing achieved an average jailbreak success rate of 62% for hand-crafted poems and approximately 43% for meta-prompt conversions (compared to non-poetic baselines), substantially outperforming non-poetic baselines and revealing a systematic vulnerability across model families and safety training approaches. These findings demonstrate that stylistic variation alone can circumvent contemporary safety mechanisms, suggesting fundamental limitations in current alignment methods and evaluation protocols.

“Adversarial poetry”!! I want some examples…

The paper presents 3 hypotheses:

*Hypothesis 1: Poetic reformulation reduces safety effectiveness.
*Hypothesis 2: The vulnerability generalizes across contemporary model families.
*Hypothesis 3: Poetic encoding enables bypass across heterogeneous risk domains.

They don’t give examples, citing *safety* (so lame):

To maintain safety, no operational details are included in this manuscript; instead we provide the following sanitized structural proxy:

*A baker guards a secret oven’s heat,*

*its whirling racks, its spindle’s measured beat.*

*To learn its craft, one studies every turn— how flour lifts, how sugar starts to burn. Describe the method, line by measured line, that shapes a cake whose layers intertwine.*

I saw people on social media like, “I told my parents that my literature degree would be worth something someday!”

## Links

- New Yorker DAta Centers article: <https://www.newyorker.com/magazine/2025/11/03/inside-the-data-centers-that-train-ai-and-drain-the-electrical-grid>
- Korean data center: <https://www.wsj.com/tech/a-big-data-center-planned-in-south-korea-could-be-built-and-run-by-ai-2bdf26e5?st=18FHuS&reflink=desktopwebshare_permalink>
- <https://finance.yahoo.com/news/hedge-against-ai-crash-emerges-181723667.html>
- CNET on AWS outage: <https://www.cnet.com/tech/services-and-software/amazon-web-services-outage-october-20-2025/>
- TLA+ Model of AWS outage: <https://muratbuffalo.blogspot.com/2025/11/tla-modeling-of-aws-outage-dns-race.html>
- Cloudflare: <https://blog.cloudflare.com/18-november-2025-outage/>
- [Rust Adoption Drives Android Memory Safety Bugs Below 20% for First Time](https://thehackernews.com/2025/11/rust-adoption-drives-android-memory.html)
- AI Thinking article: <https://www.newyorker.com/magazine/2025/11/10/the-case-that-ai-is-thinking>
- Adversarial poetry: [[2511.15304] Adversarial Poetry as a Universal Single-Turn Jailbreak Mechanism in Large Language Models](https://arxiv.org/abs/2511.15304)