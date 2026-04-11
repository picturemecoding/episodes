# Vibe Coding: the Good, the Bad, the Ugly

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

July 20th, 2025

## Guests

- Kevin Fahey: vibe coded a construction web app. Has used various low-code, no-code tools in the past (Azure Logic Apps, Bubble, that task-thing(?), etc., as well as LLMs for a while). Kevin has also managed products for software/tech companies
- Bob Farzin: dev, data scientist, math-nerd, neural-network-paper-reader

## Vibe Coding Quotes

- Karpathy’s widely quoted: “The hottest new programming language is English” on twitter in 2023
- Karpathy more recently (Feb 2025):
  + "There’s a new kind of coding I call ‘vibe coding’, where you fully give in to the vibes, embracing exponentials, and forgetting that the code even exists."
  + "It's not really coding - I just see things, say things, run things, and copy-paste things, and it mostly works."
  + and "not too bad for throwaway weekend projects"
- Simon Willison: "If an LLM wrote every line of your code, but you've reviewed, tested, and understood it all, that's not vibe coding in my book—that's using an LLM as a typing assistant.

Full Twitter*: There's a new kind of coding I call "vibe coding", where you fully give in to the vibes, embrace exponentials, and forget that the code even exists. It's possible because the LLMs (e.g. Cursor Composer w Sonnet) are getting too good. Also I just talk to Composer with SuperWhisper so I barely even touch the keyboard. I ask for the dumbest things like "decrease the padding on the sidebar by half" because I'm too lazy to find it. I "Accept All" always, I don't read the diffs anymore. When I get error messages I just copy paste them in with no comment, usually that fixes it. The code grows beyond my usual comprehension, I'd have to really read through it for a while. Sometimes the LLMs can't fix a bug so I just work around it or ask for random changes until it goes away. It's not too bad for throwaway weekend projects, but still quite amusing. I'm building a project or webapp, but it's not really coding - I just see stuff, say stuff, run stuff, and copy paste stuff, and it mostly works. - Andrej Karpathy*

Source: “[Will the future of software development run on vibes?](https://arstechnica.com/ai/2025/03/is-vibe-coding-with-ai-gnarly-or-reckless-maybe-some-of-both/)” Ars Technica

## Good

- Stuff works!
- Gets around a lot of boilerplate and boring automation work.
- One of the ironies of “vibe coding” to me is that you seem to inevitably reach a point where you’re specifying your system with more and more detail, or in many steps.

## Bad

- Sometimes it’s the wrong stuff that’s working :(
- Often distracting
- Frequently you end up spending a lot of time futzing with the tool rather than producing code
- You can produce PRs faster than people can review them (i’ve seen as many as 27 codex-produced PRs waiting for review for a team of about 5)

## Ugly

- It’s a shitload of stuff :((
- They can do wrong stuff in a totally plausible way.
- Tools will sometimes reinsert code that you intentionally removed because it’s still part of their context.
- Using a metric fuck ton of electricity and causing chaos in the engineering profession.

## References

- “[Andrej Karpathy: Software Is Changing (Again)](https://www.youtube.com/watch?v=LCEmiRjPEtQ)”
- [Vibe scraping and vibe coding a schedule app for Open Sauce 2025 entirely on my phone](https://simonwillison.net/2025/Jul/17/vibe-scraping/)
- “[Will the future of software development run on vibes?](https://arstechnica.com/ai/2025/03/is-vibe-coding-with-ai-gnarly-or-reckless-maybe-some-of-both/)” Ars Technica

## Experiences

### Bob:

- I was somewhat late to even using LLMs. Seemed like a lot of hype.
- I tried Copilot (starting in September 2024) didn’t really love it. Seemed like a good idea in concept.
  + Sidebar: Many years ago (2019?) I did talk to a friend of mine about building a language model that scraped all the python off of github on public repos, learned to generate blocks of code, ran an eval() to be sure they were valid and then could take a doc string and produce the code you wanted as a continuation. This was a seq2seq model. Before the scaling of transformers was even a thing. It just seemed “too hard” to get it to produce correct code.
  + Also experimented at some point with char level text generation and code. It was a mess - I could not get the NLP models to figure out how to close parentheses or keep straight what they wanted to do. It would quickly just spin off to generating nonsense then stop suddenly. I was doing this all on a local machine and figured it just not could be done.
- Next iteration was using the Web version to generate some code. I had a paid subscription to ChatGPT and Claude and was using that in March to explore some possible business ideas. I heard I could ask for code and began with that interface to outline what I wanted. The code that came back was pretty close to what I was looking for.
- With help from a friend of mine, I set up an API key and Claude Code. I have been using it ever since. Some things have been crucial.
  + Consider the coding bot to be a very enthusiastic intern. They are going to try to do things beyond their capacity, but believe they are great at it. I find I hold the coding back more than expected.
  + Take the extra time to generate an outline of goals and milestones
    - I often use a few other Chat interfaces to do this or try to do this. And I say right up front, “An LLM coding agent will be using this outline, please be sure to create milestones to know where we are going”
  + I take in the bigger plan and put that in my github repo, then I try to ask it to slowly plan the steps or stages one at a time and then tell me what it plans to build next. Then I have it code. So way more planning/interaction giving me confidence it will move in the right direction.
  + In general I have had very good success with this method. And I have really gone to iterating much faster than I ever would if I wrote the code myself.
    - I don’t get blocked anymore. - I am limited by my own time interacting with the tools. There is no times I say, “Yeah, that would be great but what a pain to code that! Where do I even start!” I just tell the bot to start, then if things are not right I interact with that and change it around and then allow the bot to continue to take it from there.
    - I am willing to try ideas because I am not as invested in the code. I used to think really carefully about the direction I was going to take because the building would take more time than the ideation. Now the building takes very little time. So I try more things and just pitch them if they are not right or get to a dead end.
    - I ask for way, WAY more examples and visualizations to confirm that things are going the right way- because I don’t need to build them myself.
- Where it works:
  + Matplotlib and plotting in general. These agents will go much deeper and know all the syntax to show me what data I am looking at or how it is processed. One time I said, “I don’t see what you are describing here, show me a new plot” and in addition to the plot it put in a big arrow and a label that said, “This point here!” just like an intern would do (haha)
  + Any/all boilerplate (not much in Python) seems to be built correctly from the start. For example, a pytorch training loop gets generated just as you would find the example on the web.
  + Asking questions about your code or a commit from someone else. You can just ask, “hey what is this thing doing and why”
  + Git interaction/messages. You can ask for whatever command (same in web version) which I find very helpful because I cannot remember all the functionality in git and how to merge or move around from one branch to another.
  + Transferring some style or pattern from one file to another. For example, you can manually modify a file and then say, “This is how I want to structure my other test/demo/example files, use this but make these changes and generate the new stuff.
- I take for granted things that just a few months ago would seem entirely like magic:
  + There are no syntax errors, no dropped parens or closing brackets. The imports nearly always work. No hallucinated libraries (I got that a few times with Claude 3.7 Sonnet)
  + When I say, “go fix that thing you got wrong” it knows what I mean and makes the fix.
  + Often it will run code check outputs and then fix the errors or return some evaluation like “the error went down from 3.5 to 2.7, so we are moving the right way with this analysis”
- One thing I keep saying to myself and others. No matter how much you are using these tools, you are probably not using them enough.
  + At each point that you have a request, there are so many directions you can take that and explore. The cost is small and so it is worth taking those steps. Some questions:
    - Did you try multiple different queries?
    - Did you try to ask one LLM about how to prompt another?
    - Did you change your system prompt to modify behavior?
    - Did you give it (or not give it) access to your code base to ask a question?

### Kevin

- Adopted lo/no-code tools over the years primarily as a means to get around bottlenecks and move faster.
- First started building prototypes in PowerPoint. I didn’t have formal design experience and didn’t have access or training with Illustrator (and other design tools). Powerpoint enabled me to advance ideas without going through designers.
- Eventually found prototyping tools like Balsamiq and Axure, which give me the ability to do low- and high-fidelity prototyping with more complex interaction simulation. With Axure in particular, had multiple customers surprised that the web application they were interacting with wasn’t “real”.
- Around that time also started using Optimizely, which enabled live experiments on production websites by simply adding one line of code to the HTML. I could modify text and buttons, add new sections, change links, and more using their WYSIWYG interface. The tool would then serve my test based on whatever split I determined and analyze the results based on the specified success events.
- Tools like IFTTT came into the market and offered the ability to start connecting production applications together and initiate event actions. Service integrations are available out of the box in their UI, so I could do things like setup listeners in a service (e.g. Gmail) that kickoff actions in other services (e.g. SMS) when something specific happens.
- Bubble.io was the entry point to creating a complete production web application. For the first time I was able to create a database, create an interface with inputs, calculate and send user entered values into that database, and do different things in the application based on what was in the database.
  + As a complete standalone app I think it probably worked fine, but integrating it into other systems required dev expertise and refactoring.
- When LLMs started flooding the market, I started experimenting with different ways to process information through the models. I used ChatGPT to create a small python application that took a bunch of information from an Excel file, produced a prompt and iterated through each row in the Excel file, took the response from the LLM and created a new Excel file with the results. The early results weren’t great, but it felt powerful.
  + Worth noting that Erik had to help me debug that python file.
- I started using Cursor and Claude Code to see if I could automate some annoying back office tasks for myself. Within a few hours, I had an application that captured my workflow, sent documents out for signature via API, received and processed webhooks from that API, and ingested documents from that API. It felt insanely magical and addicting. I started throwing in whatever feature I could dream up.
  + FWIW, I don’t believe the mega-app I’ve been building is necessarily sane or the optimal way to use these tools. Smaller, self-contained apps are without a doubt a better use case.

Some general observations:

- + The quality and speed improved when I started requiring a detailed design plan and clarifying questions before writing any code. It rushes into code and makes a ton of assumptions if you don’t force it to stop and ask questions.
  + Started adding User and Project rules, which seem to help a bit as well. It doesn’t completely adhere to the rules at all times (likely U shaped context window issue), but it feels like I’m reminding it of my preferences less often.
  + It is WAY too agreeable and eager to please. Every request, no matter how stupid, gets a “Great idea!” or “You’re absolutely right!”. I intuitively know a meaningful number of ideas I throw out there are either underdeveloped or just bad. I would prefer that it argue with me a bit and push back. An occasional “Why would you do that?” or “That’s lame… here’s why” would be welcomed.
  + Tweaking UI elements was initially the most frustrating aspect.
    - Creating a global components/style guide page helped. I include the name of the component and it immediately knows what I have in mind without having to re-write the entire narrative on what I want. This also helps with the tendency to create completely new components every time I’m starting work on a new page or section.
  + The ability to ask the tool for documentation for how something works is incredible. Within seconds it kicks out a summary that is written in a way that gives me the exact insight I need. They tend to be concise and not overly technical.
  + Cursor has a major unaddressed UX issue with the Reject function, which effectively deleted a few hours of work.
  + It once deleted the majority of a file accidentally and it couldn’t figure out how to recover it.
  + I also occasionally find it gets “distracted”. I’ll ask it to do something specific and in the process it discovers something that it deems an issue and starts working on it. I’ve had to stop and redirect it numerous times.

### Mike

- Been using Copilot for a couple of years, no vibes there. Also Gemini Code Assist
  + I use these mostly in chat mode now, i find the autocomplete distracting
- Pretty good luck with Cursor, primarily for React/Typescript, which i don’t know that well.
- Using Codex currently. I’m impressed by it, but have had mixed results. The group i’m working with often does entire PRs for new features.
- I’ve recently been “vibe querying”, where i start conversations with various models about how to improve SQL or Spark code. I really like this mode, and it’s absolutely killer for explaining query plans, especially Postgres query plans which are hard to read. I’ve found that it often gives me insight into why a query is not as efficient as it should be and how to simplify and restructure. However:
  + You can tell that the model is heavily influenced by training data. For example, i’ve had queries with joins where the best option really is a nested loop, but since every join optimization article ever written talks about making indexes and trying to get merge or hash joins, the AI will insist that i need to make indexes.
  + The models don’t really understand the logic. I had one case where i used Codex to change a query involving a UNION, and in the process of rewriting it, the model switched the predicates on the two parts being unioned.