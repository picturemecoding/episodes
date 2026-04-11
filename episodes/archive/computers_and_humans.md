# Ubiquitous Computing

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

Mike Music: Geese - *Getting Killed*

I freakin’ love this album and I don't know why. The band has also exploded for no apparent reason, becoming something of a TikTok sensation and being called “the first great Gen Z indie band”. There’s nothing super innovative or surprising about the music, but it’s got a notably different vibe than most recent indie rock. It’s kind of fun, a little funky, and the singer (Cameron Winter, who also had a solo album earlier this year) has a very distinctive and strong voice.

## Introduction

Did you know there is a group of developers who have rewritten [coreutils](https://github.com/uutils/coreutils) in Rust? This venerable collection of utilities for the shell includes over a hundred commands familiar to our users such as *ls* and *mv* and *rm*. As we were discussing recently, these tools were first created in the 70s by the Unix gang over at Bell Labs. But it made me wonder: these tools, and the shell itself must be *ergonomic* because we’re still using them over 50 years later. If they weren’t ergonomic, they would probably have been replaced by something else, right?

We could also frame it a different way: why didn’t we move past the shell? Why are we *stuck* in a method of managing our computers invented and popularized in the 60s and 70s? We could imagine that people like us would have adopted some entirely mouse-and-desktop-based way to do software development (because we’re mostly using text editors that are desktop-based).

A different set of questions is around what other ways of managing and interacting with computers could be out there? If the mouse-and-desktop interface were almost useful but incomplete, what else might have been better? We are still doing terminal hard-core, but what other possibilities for human-computer interaction are out there lurking in the wings, waiting to take over?

Xerox Parc

This conversation comes out of an area of study known as “human-computer interaction”. The mouse and the “desktop” (as a metaphor and an implementation) were actually invented at a place in Silicon Valley called Xerox Parc.

Things invented at [Xerox Parc](https://en.wikipedia.org/wiki/PARC_%28company%29):

- Laser printing
- Ethernet
- The GUI (and the desktop-metaphor)
- Electronic paper
- The mouse
- OOP and small-talk invented there

“Ubiquitous Computing”

[Mark Weiser](https://en.m.wikipedia.org/wiki/Mark_Weiser) coined the term “[Ubiquitous computing](https://en.wikipedia.org/wiki/Ubiquitous_computing)” around 1988 when he was Chief Technologist at Xerox Parc

In addition to his work in the field of computer science, Weiser was also the [drummer](https://en.m.wikipedia.org/wiki/Drummer) for the [avant-garde](https://en.m.wikipedia.org/wiki/Avant-garde)/[experimental](https://en.m.wikipedia.org/wiki/Experimental_rock) [rock](https://en.m.wikipedia.org/wiki/Rock_music) band, [Severe Tire Damage](https://en.m.wikipedia.org/wiki/Severe_Tire_Damage_%28band%29), which was the first band to broadcast live over the Internet

Weiser’s 1991 essay “[The Computer for the 21st Century](https://web.archive.org/web/20141022035044/http%3A//www.ubiq.com/hypertext/weiser/SciAmDraft3.html)”

From the opening:

The most profound technologies are those that disappear. They weave themselves into the fabric of everyday life until they are indistinguishable from it.

Consider writing, perhaps the first information technology… Candy wrappers are covered in writing. The constant background presence of these products of "literacy technology" does not require active attention, but the information to be conveyed is ready for use at a glance. It is difficult to imagine modern life otherwise.

By contrast even though a ton of computers were in the wild by 1991, Wiser writes:

More than 50 million personal computers have been sold, and nonetheless the computer remains largely in a world of its own. It is approachable only through complex jargon that has nothing to do with the tasks for which people actually use computers. The state of the art is perhaps analogous to the period when scribes had to know as much about making ink or baking clay as they did about writing.

[Thus], we are trying to conceive a new way of thinking about computers in the world, one that takes into account the natural human environment and allows the computers themselves to vanish into the background.

Question: does this appeal to you? An environment rich in computers where they “vanish into the background”? (There’s always a sci-fi element to when we start imagining these possibilities, no?)

Philosophical underpinnings:

Such a disappearance is a fundamental consequence not of technology, but of human psychology. Whenever people learn something sufficiently well, they cease to be aware of it. When you look at a street sign, for example, you absorb its information without consciously performing the act of reading. Computer scientist, economist, and Nobelist Herb Simon calls this phenomenon "compiling"; philosopher Michael Polanyi calls it the "tacit dimension"; psychologist TK Gibson calls it "visual invariants"; philosophers Georg Gadamer and Martin Heidegger call it "the horizon" and the "ready-to-hand", John Seely Brown at PARC calls it the "periphery". All say, in essence, that only when things disappear in this way are we freed to use them without thinking and so to focus beyond them on new goals.

What does “ubiquitous computing” look like for Weiser? He gives these XeroxParc- implemented examples:

Ubiquitous computers will also come in different sizes, each suited to a particular task. My colleagues and I have built what we call tabs, pads and boards: inch-scale machines that approximate active Post-It notes, foot-scale ones that behave something like a sheet of paper (or a book or a magazine), and yard-scale displays that are the equivalent of a blackboard or bulletin board.

- “Tabs”
- “Pads”
- “Boards”

Imagine walking around a building and all of your documents “follow” you from room to room? Is this so different from sharing my screen on a tele-meeting?

The PARCTab is an experimental mobile computing device as an early experiment in ubiquitous computing (UbiComp). Its appearance resembles a personal digital assistant (PDA). Its functionality depends on the user's location, by receiving location-specific information via infrared sensors from gateway nodes installed in a particular location.

It has a touch screen, stylus, and handwriting recognition. Xerox designed the similar and larger PARCPad. Both devices were developed around the same time as the Apple Newton.

Question: does the mobile phone count? No, according to MIT’s [Project Oxygen](https://en.wikipedia.org/wiki/Ubiquitous_computing#Examples):

In the future, computation will be human centered. It will be freely available everywhere, like batteries and power sockets, or oxygen in the air we breathe… We will not need to carry our own devices around with us. Instead, configurable generic devices, either handheld or embedded in the environment, will bring computation to us, whenever we need it and wherever we might be. As we interact with these "anonymous" devices, they will adopt our information personalities. They will respect our desires for privacy and security. We won't have to type, click, or learn new computer jargon. Instead, we'll communicate naturally, using speech and gestures that describe our intent...

What about VR? Wiser bashes VR, argues it’s the *opposite* of what he’s talking about:

Perhaps most diametrically opposed to our vision is the notion of "virtual reality," which attempts to make a world inside the computer. Users don special goggles that project an artificial scene on their eyes; they wear gloves or even body suits that sense their motions and gestures so that they can move about and manipulate virtual objects. Although it may have its purpose in allowing people to explore realms otherwise inaccessible -- the insides of cells, the surfaces of distant planets, the information web of complex databases -- virtual reality is only a map, not a territory. It excludes desks, offices, other people not wearing goggles and body suits, weather, grass, trees, walks, chance encounters and in general the infinite richness of the universe. Virtual reality focuses an enormous apparatus on *simulating* the world rather than on invisibly *enhancing* the world that already exists.

VR in the news with Björk's 2015 album *Vulnicura* rendered as a [VR experience](https://www.npr.org/2025/09/23/nx-s1-5543338/bjork-vr-vulnicura-remastered) (?):

Björk's project arrives in its remastered version at an inflection point for the VR industry. A recent report from the market intelligence company International Data Corporation (IDC) forecasts a compound annual growth rate of nearly 40% between this year and 2029. At the same time, more than half of the game developers who participated in a recent survey about VR said the industry is stagnating.

"I continue to believe VR has tremendous creative potential, particularly with regard to live performance," said Charlie Fink, a producer, podcaster, author, and Chapman University lecturer in the immersive technology space. "That said, VR has not achieved the commercial success everyone thought was possible ten years ago."

[Designing Calm Technology - Computer Repair Blog](https://www.karlstechnology.com/blog/designing-calm-technology/)

​​Designs that encalm and inform meet two human needs not usually met together. Information technology is more often the enemy of calm. Pagers, cellphones, newservices, the World-Wide-Web, email, TV, and radio bombard us frenetically. Can we really look to technology itself for a solution?

But some technology does lead to true calm and comfort… [The pen, the shoe(!)] We believe the difference is in how they engage our attention. Calm technology engages both the *center* and the *periphery* of our attention, and in fact moves back and forth between the two.

We use “periphery” to name what we are attuned to without attending to explicitly.

…

A calm technology will move easily from the periphery of our attention, to the center, and back.

## Ambient Computing

Apparently, ubiquitous computing has evolved into “ambient computing” in some circles, and also “ambient intelligence”, which is a further evolution that analyzes the environment and predicts how to adjust things.

[Ambient computing has arrived: Here's what it looks like, in my house | ZDNET](https://www.zdnet.com/article/ambient-computing-has-arrived-heres-what-it-looks-like-in-my-house/)

Instead [of ubiquitous computing], we're being drawn to another wave, one that takes Weiser's ubiquitous computing vision and mixes it with the Internet of Things (IoT), machine learning, and the hyperscale compute cloud to deliver what's being called 'ambient computing'. As an alternative to traditional computing models, ambient computing takes its cue from musician Brian Eno, who in coining the term 'ambient music' for his slow compositions, described it as something that "must be as ignorable as it is interesting."

The writer gives the example of his home heating system.

He then says that ambient systems present information in different ways. You could have a weather indicator on your fridge that shows blue for cloudless skies or something: “An ambient interface needs to be glanceable.”

Question: Has ubiquitous computing been achieved?

Some of the stuff that gets promoted as a ubiquitous or ambient computing tech:

- Smartphones/tablets
- IoT stuff, including Alexa, Google Home, etc.
- Wearables
- Home automation/alarm systems
- Smart city
- Augmented reality
- mmWave (millimeter wave communication)

The program from the International Symposium on Wearable Computing

[Program Overview](https://www.ubicomp.org/ubicomp-iswc-2025/program/)

I think ubiquitous computing has *not* been achieved in the way that Weiser envisioned, in part because so much of technology is focused on grabbing our attention or collecting data. Things on the periphery can’t sell ads or engage us to learn our behaviors.

Question: How Do You Program These Things?

- User interfaces are different, either simpler or voice or sensors
- Computing gets offloaded to cloud (eg, there are Lambda functions for Alexa)
- Embedded stuff, often without a lot of security

[Toward Ubiquitous Operating Systems: Lessons from the Field](https://cacm.acm.org/opinion/toward-ubiquitous-operating-systems-lessons-from-the-field/)

This article is about how all devices in an area could be using a “ubiquitous operating system” and automatically communicate with each other. They give the example of a hospital room with half a dozen or more devices with computers inside them *and* an ambulance-delivering a patient to the room where the ambulance may also be running the ubiquitous operating system.

Developers can define easilymulti-device activities, leaving low-level functions to ubiquitous operating systems. From the perspective of applications, cross-device communication and hardware access are no longer integrated as part of each application, but as a system-level infrastructure serving all applications.

[https://en.wikipedia.org/wiki/Fuchsia\_(operating\_system)](https://en.wikipedia.org/wiki/Fuchsia_%28operating_system%29)

<https://www.harmonyos.com/en/>

Optional: Licklider stuff

<https://en.wikipedia.org/wiki/J._C._R._Licklider>

[Man-Computer Symbiosis](https://groups.csail.mit.edu/medg/people/psz/Licklider.html)