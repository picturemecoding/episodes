# Scale 23

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

March 5-8 2026, Pasadena CA

## Erik

Day 1-2 wed/thurs

Behind the scenes: I hung out in the Noc from Wednesday to Sunday with Rob and a few other people as well as a rotating cast of characters wandering in and out. They were flashing openWRT routers on Wednesday and the whole operation was pretty interesting. They use nix to deploy their servers, including three servers which function as border routers and two for DHCP and other operations they want to deploy, and then they have this fleet of volunteers who show up to run miles of network cable and to setup the switches and routers.

This year they had three border routers which Rob patiently explained to me formed a triangle, so if any one goes down, the other two will still handle routing. There are a good number of switches (every room in the conference has its own network switch) and then almost 200 access points strewn about all over the place. The keynote ballrooms, for example, may have a dozen or more openWRT routers.

The trick is actually monitoring all of this: is everything running? Is anything overloaded or slow? How’s the wifi coverage?

They had used this massively overpowered server with 256gb of RAM and it does DHCP assignments, but they had also forwarded logs to this thing from all of the switches and access points using rsyslog. So you go into this one logging directory on the machine and there’s like 50gb of logs on it on the first day.

Anyway, one of the volunteers from recent years is a guy named Erik Reinert, and he’s a famous software/devops streaming person! You can actually find streaming software dev at TheAltF4on youtube and twitch (which we can include links for). Last year, working at Scale he spun up a modern observability stack like you would find at contemporary companies: so when I showed up this year they already had Prometheus metrics (via the Mimir project) and they had aggregated logs from Grafana’s Loki project and all of this was accessible behind a Grafana dashboard. But the staff are mostly networking, infra, and Linux people so they didn’t have a lot of experience with it. Aside from Erik, I was probably the only other person on staff with hands-on experience with that stuff.

So I was like, “cool, ssh into linux machines and mess with Grafana: I can help with that.” I helped debug the observability stack, and I worked on some Grafana visualizations and getting logs into Loki. Erik made a Grafana playlist that showed various health metrics for the network, inventory that’s online and as well as network health, throughput, etc. This playlist was a big hit at the conference and the people at the Grafana booth down on the expo hall floor even ended up displaying it proudly at their booth! I was like, “hey, I made that panel!” when I went to talk to them.

One thing I noticed in the Noc: these super experienced and incredibly capable people were typing questions into Claude Code when they needed to debug something. I guess I never realized how much I’ll search the internet for information I don’t have immediately in my brain. At first I was surprised but it just took over from Google I think probably because you can ask it follow-up questions and pursue solutions with it. I think it’s a hit because it’s often *more expedient for debuggin*g.

I ended up going to a few talks on Thursday afternoon, the most interesting one being a guy who worked at Anthropic on the platforms team. They provide kubernetes dev pods for people to do software dev. Anthropic is a big Nix user (in fact, they sponsored the conference because of the Planet Nix and various Nix-related content at the conference). The talk was given by Sam Fu from Anthropic. He said he’s not a Nix expert, but who’s been doing developer experience for 9 years. He works on the dev Acceleration team at Anthropic: to accelerate the productivity of devs. Their workflow is they launch and then ssh into a kubernetes pod, which is dev env, a dev shell, to work on stuff and Sam’s team makes this happen via one nix command. But they ran into some issues he had to hack around and he walked us through how he solved them.

I went to another Nix talk that afternoon where an engineer named Morgan Helton at a company called Flox was talking about short-cutting some parts of the docker build image flow by using Nix to directly emit an OCI-spec-compatible layers and manifest.

The nix people are doing pretty interesting stuff.

Rob had given me an old Thinkpad to put NixOS on so that I could contribute to the scale-network repo. Their repos are all opensource, by the way, if you want to check them (link socallinuxexpo GH org). But they use Nix for everything so Rob said, “you should use this computer; put NixOS on it and then you can run our codebase.”

Verdict: I really liked Nix. Make a one-line change to a config file, run nixos-rebuild switch and the entire system is rebuilt with all dependencies and configurations loaded and if it doesn’t work, fallback to the previous. It was extremely good (but there is a decent learning curve). However, I only lasted one daywith this machine because I hated the screen so much. So now I’m constantly thinking to myself, “I need to get a new computer so I can put Nix on it.” There is just absolutely *no comparison* when it comes to configuring systems: it’s deterministic, repeatable, and super cool.

### Stormy Peters “Is AI Killing Open-Source Software”

Friday morning.

The best talks I went to at this conference were all keynotes and this talk by Stormy Peters was super interesting and I’m fascinated by this discussion. Mike, we’ve talked a lot about open-source software. Stormy Peters brought a lot of statistics and examples for discussion.

- 84% of devs now use AI tools now (76% in 2024).
- 66% of devs frustrated by AI tools being "almost right."
- 45% say debugging AI code takes more timethan writing it.

She cited a Tidelift study of open source maintainers that reported:

- 60% are unpaid
- 60% consider quitting
- 44% cite burnout

And now popular projects are getting inundated with slop contributions which they take different strategies to mitigate. She offered this quote:

"It used to be that we mark GH issues as "good first issue" and ambitious young engineers would show up. Now we file something as good first issue" and lin less than 24 hours get absolutely inundated with low quality vibe-coded slow that takes time away from doing real work. This pattern of turning slow into quality code through the review process hurts productivity and hurts morale."

Some projects ban AI contributions entirely (she gave Gentoo as an example). Other projects, quarantine it by taking an especially long time to evaluate these PRs. Other projects will limit the *size* of a contribution by a first-time contributor to prevent massive slop PRs.

She said one opensource project received a massive refactor PR which the contributor siad they’d used an LLM for and the maintainers said, "well, instead of merging this, why don’t you give us the prompt and we'll try to recreate it".

She said that it’s uneven whether people *admit* they’ve even used it. In another study, she said:

- 95.2% of devs use AI to make Pull Requests.
- 29.5% of devs disclose that they use AI in a PR
- 80.5% of Claude users disclose their AI use (!)
- 9.0% of Github copilot users disclose it.

She didn’t really *answer* her own question, except to say “probably not”, but then she said something pretty fascinating near the end of her talk: small tools, libraries, stuff like that doesn’t need to exist as independent packages if we can have Claude or Copilot write them. She said, “There is no need for small dependencies. Those types of projects are probably going away.” On the other hand, large successful projects (she gave kubernetes as an example) are not going to be replaced by AI-coded projects because they’re too big and have too much momentum.

I asked her afterwards, this is a near-term thing? Eventually we may have the capacity to produce a kubernetes, right? (Even though I can’t imagine anyone wanting to.) And she conceded, yeah, that could happen.

Weekend:

Expo floor: I talked for a while to a product person at Microsoft and then to a DevRel person at Github (they share a booth). The DevRel person is named Ashley and she was telling me about a Github competitor to Claude Code, the copilot CLI. (rabbit hole on BMO project…?)

### Cindy Cohen Executive Director of the EFF

She was a defense attorney for the landmark court case Bernstein v Dept of Justice in the 90s.

There was a guy named Dan Bernstein who wanted to publish some cryptographic protocols on the internet. He was a student in a mathematics Ph.D program. He was told that if he published cryptographic protocols on the internet, that would go to jail as an arms dealer.

So the EFF files this case in 1994. Cindy Cohn argued at the time that the government only wants cryptographic protocols they can break out in the wild. (They want to keep the best stuff secret).

The US government classified encryption software as a MUNITION (it could not be exported from the United States; and publication counts as export). She said that PGP Creator Phil Zimmermann faced criminal investigation for making his code available. Her argument was that this is a violation of the 1st amendment.

Eventually the attorney she argued against invited her to come to Washington DC to help craft new legislation around encryption regulation.

She talked about other examples of the government wanting access to phone and internet systems.

## Mike

### Embedded Linux

Interesting talk, if a bit out of my comfort zone. As the name implies, about using Linux in embedded devices. Some things i learned:

- Once the ROM bootloader runs, there’s usually a “secondary program loader”. This is necessary because the DDR SDRAM isn’t actually functional at first, so the SPL has to “train” the DDR so it can load the real bootloader, usually something like U-Boot
- Embedded systems use something called “Triple modular redundancy”, because they are often in environments where they can’t be rebooted (or might even be affected by radiation). Basically it means there are three modules that do that same function and the system chooses the “right” output by majority vote.
- All of the software and hardware necessary to go from inert plastic and silicon thingy to device that can accept inputs is called the BSP, or board support package.
- There are tools for creating custom embedded Linux distributions, most common are Buildroot and Yocto
- Embedded Linux uses a device tree like standard Linux to identify and configure devices on boot.
- Embedded engineers have 3 options for the hardware they start with: a fully functional device, a single board computer (Raspberry Pi, Beaglebone), or making their own board. Slang for the last approach is “chip down”.

### Russinovich

Russinovich is the CTO of Azure. He started with a slightly sales-pitchy catalog of his and Microsoft’s contributions to Open Source.

He was trying to earn our trust! So he gave a bunch of linux credentials for himself and MS:

- Several MS Engineers became top-5 contributors to Linux
- MS has its own linux distro
- 65% of customer compute on Azure cores are Linux (customers primarily use as a linux platform)
- Microsoft's Cosmic is one of the largest k8s clusters in the world (on AKS) (MS 365 runs on top of this)
- 3 million PRs MS engineers making on OSS projects
- Top 100 orgs listed by the linux foundation shows MS at the top (rhel => 2, google => 3, etc.)
- KEDA, dapr, radius, drasi, VSCode (most used code editor in the world)

Funny side note: he kept referring to Microsoft’s “evil empire days” which was pretty funny and went over well with the audience, I thought.

97% of codebases contain OSS dependencies.

The main focus of the talk is supply chain security in open source, because “OSS is a delivery channel for attackers.” He mentioned several open source vulnerabilities, some of which we’ve talked about on the pd (log4j, lza). He also mentioned something called Shai-Hulud 2.0, which i had not heard of.

In 2025 alone, devs downloaded more than 42 million vulnerable versions of log4j, representing 13% of all log4j downloads worldwide.

Pytorch 2022: pkg manager confusion attack:

It had an “internal”-only package someone found out about.

pip would pull a public version of a higher number over internal version.

Someone shipped a public compromised version of this lib.

And so Pytorch loaded this compromised public take-over of the package name,

Shai-hulud 2.0: Aggressive automated, and fast spreading

- an npm maintainer compromised through a phishing attack (no MFA)

- pkgs maintained by that dev were compromised

- attacker went viral because anyone who pulled the package would then compromise their own npm signing keys

- exponential spread: worm through maintainers

He then talked about AI and how that’s expanding the capabilities of both white and black hats.

“Supply Chain Attack Secretly Installs OpenClaw for Cline Users”

- Cline is an assistant thing from NPM ecosystem

- a postinstall hook was added to Cline which was the command “npm install -g openclaw”

- this was made possible because Cline themselves have this Claude Issue Triage tool which itself was vulnerable to *prompt injection attacks*, which allowed *anyone to publish* Cline packages! (See [Adnan Khan’s blog for more](https://adnanthekhan.com/posts/clinejection/)) (The technical details are pretty involved…)

The [articles about it](https://www.darkreading.com/application-security/supply-chain-attack-openclaw-cline-users) were like, “geez, apparently people aren’t trying out openclaw quickly enough for somebody.”

Favorite line: "Not looking at the code is not the flex you think it is"

- AI using old versions of APIs that are maybe insecure.
- AI Getting old versions of deps maybe insecure.

Anthropic set Claude loose on OSS projects and within a few days they found loads of vulns

Set it loose on Mozilla and found ~148 vulns (within a few days). Google last year saw 75 vulns all yearexploited.FF is one of the most secure OSS codebases there is: so Anthropic/Claude is tearing down the doors for discovering vulns.

Much talk about OpenSSF and related tools and projects

- SLSA
- Sigstore
- SCITT
- S2C2F

We apparently missed the most interesting bit of his talk, where he used an LLM to reverse-engineer some 40 year old 6502 *machine* language that was part of an article he wrote in the 80s. Cute, but also makes the point that any binary is now a much more accessible attack surface.

[erik] I liked this talk a lot. My takeaway was that the CVE storm nightmare is only going to get worse!!

### Cobol to Cursor (Brendan OLeary)

O’Leary works for an AI startup called [Kilo Code](https://kilo.ai/).

The key idea in this talk is that people have been whinging about “moving up the stack” at least since Grace Hopper introduced the first compiler (ie, people resisted assembly -> compiled, compiled -> interpreted, bare metal -> VM, etc.). He made the same analogy that we’ve used, courtesy of Bob, that looking at source code will eventually be as common as looking at compiler output. In the end, another “I for one welcome our new overlords” talk.

The history part of the talk was a little disappointing. He talked about Hopper’s early years, her work on the first compilers, and her time in the Navy. But he didn’t do much research on her actual work.

Some advice he offers about LLM coding

- Be explicit/provide context/set constraints
- Human review of planning and research. Again something we’ve discussed a bit–using the context and specs as reviewable artifacts.
- He mentioned someone named Dex Horthy, but i forgot the context now

O’Leary wrote a thing called PinchBench, which apparently benchmarks OpenClaw? It apparently went slightly viral.

Somebody in the audience mentioned another podcast called [Advent of Computing](https://adventofcomputing.com/), that might be of interest to PMC listeners.

### Sidebar: AI Stuff

General talks I went to about AI. Some demos. And then Erik Reinert’s demo. Docket. My BMO project which shamelessly copied Docket so I could understand it.

### Plugin & Play Postgres with CNPG-I (Sharif Shaker)

I went to this one partially because i’m a database nerd with some k8s experience, and partially because i follow the company the speaker works for ([EDB Postgres](https://www.enterprisedb.com/))

The talk was about CloudNativePg (CNPG) and specifically the CNPG-I, plugin system. Roughly speaking CNPG is an “operator” that lets you run Postgres under K8s in a sensible way. Plug-ins let you add functionality, like automated backup.

A plug-in can extend either (1) the CNPG operator, or (2) the “instance manager”, the part of CNPG that actually runs postgres.

A plugin can be deployed either as a sidecar or stand-alone. They communicate with the operator via gRPC.

There are standard interfaces that plugins must implement though some are optional.

Special handling of instance manager plugins is required.

Barman cloud is an established plug-in for doing backups.

The spark did a demo of an MCP server as sidecar.

### Software distribution now and then - Comer

[Douglas Comer - Wikipedia](https://en.wikipedia.org/wiki/Douglas_Comer) is a professor of computer science at Purdue and one of the pioneers of the TCP/IP protocols. I own his book on hardware architecture, but not his better known books on TCP/IP. Also created XINU and several books on OSes.

The talk focused on how the distribution of software has changed during his career, and how many of the architectural decisions of the Internet made modern distribution possible.

As a grad student was told: "Networking will never be part of computing"

Some fun facts of pre-Internet distribution

- Box of 2000 cards weighed 11 pounds
- 10.5 mag tape weighs 2.2 lbs, maxed at 140mb
- Talks about using tar as an actual tape archiver
  + Tar needed block size argument because some computers didn't have enough memory to load big blocks
- Bytes/ lb cards-14545, tape-63,636,364, flash 439b
- There were small “mailer tapes” for sending programs to other people.

Internet stuff

- Original networks only connected same types of computers (ie IBM to IBM)
- Endianness was a problem (i could tell a lot of the audience didn’t know what he was talking about). IBM computers were big-endian but the computers he was doing research on were little-endian.
- The Internet favored services outside the network. The phone company model was to put new services into the network equipment (eg, call forwarding took 5 years to implement because it required changing every network switch).
- NAT kept phone companies from charging per computer rather than per connection.
- Before DNS used broadcast to find servers. Basically sort of like the Ethernet model (hey, does anyone out there have *this* address).
- End to end transport vs link by link. Before TCP it was assumed that every hop in the network would have to verify receipt of packets.
- Adaptive retransmission was an important innovation to keep from swamping networks.
- The internet model made modern software distribution possible
  + Can connect any computer to any other computer regardless of manufacturer
  + Flat fee for connections (no “long-distance” model)
  + Easy to add services
  + DNS made routing easier
  + Efficient enough for modern file sizes.

## Links

- [Scale conf](https://www.socallinuxexpo.org/scale/23x)
- [Scale-network repo](https://github.com/socallinuxexpo/scale-network)
- Erik Reinert’s [Youtube Channel](https://www.youtube.com/thealtf4archives)
- [Docket](https://github.com/ALT-F4-LLC/docket)
- [BMO](https://github.com/erewok/bmo) and [bmo-agent-setup](https://github.com/erewok/bmo-agent-setup)
- [Cline injection attack Adnan Khan](https://adnanthekhan.com/posts/clinejection/)
- [Douglas Comer - Wikipedia](https://en.wikipedia.org/wiki/Douglas_Comer)
- [EDB Postgres](https://www.enterprisedb.com/)
- [Advent of Computing](https://adventofcomputing.com/) Podcast
- [Kilo Code](https://kilo.ai/)