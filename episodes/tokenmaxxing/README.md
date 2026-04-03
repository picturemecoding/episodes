# TokenMaxxing!
## Managing a Team of Agents to Build Software

---

## Episode Summary

Inspired by Steve Yegge's January 2026 essay ["Welcome to Gas Town"](https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04), Erik and Mike dig into the emerging world of multi-agent software development. What does it actually mean to "manage a team of agents"? How did we get here so fast? And what's it actually like in practice? The episode covers the landscape of available tools, the key ideas behind agent orchestration, and Erik and Mike's personal experiences trying to get a colony of AI agents to do useful work — without losing their minds or their context windows.

---

## 🎵 Music Segment

- **Erik:** *[What are you listening to?]*
- **Mike:** *[What are you listening to?]*

---

## Historical Timeline

- **Pre-2024** — LLMs used for code completion and generation (GitHub Copilot, 2021), but agents are stateless and single-turn. No ability to run tools, hold state, or iterate.
- **Early 2024** — Research groups begin experimenting with giving LLMs access to terminals, browsers, and editors. The paradigm shifts from "code generation" to "software engineering agents."
- **March 2024** — [Cognition Labs introduces Devin](https://cognition.ai/blog/introducing-devin), billing it as "the first AI software engineer." It achieves **13.86%** on SWE-bench — far above the prior state-of-the-art of 1.96%. The demo is impressive; the discourse is explosive.
- **May 2024** — Princeton/Stanford researchers publish [SWE-agent](https://arxiv.org/abs/2405.15793) (NeurIPS 2024), an open-source agent that achieves 12.5% pass@1 on SWE-bench by introducing the idea of an **Agent-Computer Interface (ACI)** — a purpose-built interface between the agent and its tools.
- **Mid-2024** — OpenAI introduces [SWE-bench Verified](https://openai.com/index/introducing-swe-bench-verified/), a human-validated subset of the benchmark to address concerns about contamination and evaluation quality. The benchmark ecosystem matures.
- **Late 2024** — Claude 3.5 Sonnet hits **49%** on SWE-bench Verified. The pace of improvement is startling.
- **May 2025** — Anthropic releases **Claude Code**, a CLI-native agentic coding tool. It quickly becomes the most-used and most-loved AI coding tool by 2026.
- **2025** — **Cursor** ships full agent mode with multi-agent orchestration. GitHub Copilot adds agent mode. The tooling landscape explodes.
- **January 1, 2026** — Steve Yegge publishes ["Welcome to Gas Town"](https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04) and releases [Gas Town on GitHub](https://github.com/steveyegge/gastown) — a multi-agent orchestration framework built on top of Claude Code. He follows up with ["The Future of Coding Agents"](https://steve-yegge.medium.com/the-future-of-coding-agents-e9451a84207c).
- **Early 2026** — Claude Opus 4.1 scores **74.5%** on SWE-bench. The leap from 1.96% to 74.5% in roughly two years is one of the most dramatic benchmark progressions in AI history.

---

## Seminal Papers

### [SWE-bench: Can Language Models Resolve Real-World GitHub Issues?](https://arxiv.org/abs/2310.06770)
*Jimenez et al., 2023*

The benchmark that defined the field. SWE-bench presents agents with real GitHub issues from production open-source Python repositories and asks them to generate a patch that resolves the problem. It reframed evaluation of coding AI from "can it write a function?" to "can it fix a real bug in a real codebase?" — a much more meaningful bar.

> Key idea: evaluation should reflect actual software engineering tasks, not toy problems.

---

### [SWE-agent: Agent-Computer Interfaces Enable Automated Software Engineering](https://arxiv.org/abs/2405.15793)
*Yang et al. (Princeton/Stanford), NeurIPS 2024*

SWE-agent's central contribution is the **Agent-Computer Interface (ACI)** — the idea that the interface between an agent and its tools matters enormously, just as UI/UX matters for human developers. By designing purpose-built tools (search, edit, run) rather than raw shell access, they significantly improved performance.

> Key idea: don't just give agents a shell — design the interface for agents.

---

### [Introducing Devin: The First AI Software Engineer](https://cognition.ai/blog/introducing-devin)
*Cognition Labs, March 2024*

Not a traditional paper, but a watershed moment. Devin's demo — autonomously setting up environments, writing and debugging code, and browsing the web — moved the Overton window on what agent-based software development could look like. It sparked enormous debate, inspired competitors, and made SWE-bench a household name (in certain houses).

---

## Key Concepts

- **Agent Orchestration** — The practice of having one agent (a "mayor," planner, or coordinator) direct a team of other agents (coders, reviewers, testers), routing work and managing state.
- **Beads** (Gas Town) — Yegge's lightweight, Git-tracked issue tracker. Each "bead" is a JSON object with an ID, description, status, and assignee — the atomic unit of work in a Gas Town colony. Solves the problem of agents losing context between sessions.
- **Context Window Management** — The core practical challenge of multi-agent development. Each agent has a limited context window; when it fills up, the agent forgets. Hence: *TokenMaxxing* — trying to get the most done before the window runs out.
- **Agent-Computer Interface (ACI)** — The interface layer between an agent and its tools. Analogous to a GUI for humans. Good ACI design is a significant multiplier on agent effectiveness.
- **SWE-bench** — The standard benchmark for autonomous software engineering agents. Tasks are real GitHub issues; success means a passing test suite.
- **Developer Evolution Model** (Yegge) — Yegge's informal stages of AI coding adoption, from "not using AI" to "managing colonies of agents." Gas Town is explicitly designed for Stage 6-8 developers.
- **"Kubernetes for Agents"** — Yegge's framing for where the industry is heading: Claude Code is just a building block; the real action will be in orchestration layers, scheduling, and workflow management.

---

## Discussion Questions

1. **Where are you on Yegge's developer evolution model?** Are you running single agents, parallel instances, or full orchestration?
2. **What does "managing a team of agents" actually feel like?** Is it empowering? Exhausting? Both?
3. **The 1.96% → 74.5% SWE-bench arc happened in about two years.** Does that feel real to you in your daily work, or are benchmarks misleading?
4. **Gas Town requires a lot of scaffolding.** Is this the future, or is it a transitional tool that will be obsoleted by better agent runtimes?
5. **What's the right mental model?** Are agents like junior developers? Contractors? Something else entirely?
6. **Token anxiety** — how much of your cognitive load when working with agents is managing context windows? Is this a fundamental constraint or an engineering problem that will be solved?
7. **What's been your biggest failure with agents?** (The episodes where they confidently did the wrong thing for hours...)
8. **Is "vibe coding" a stepping stone to orchestrated development, or a different thing entirely?**

---

## Interesting Angles and Stories

- **The benchmark rocket ship** — Going from 1.96% to 74.5% on SWE-bench in ~2 years is one of the most dramatic benchmark progressions in recent AI history. It's worth taking a moment to sit with that number.
- **Yegge's credibility arc** — Yegge is the author of legendary software rants ("Stevey's Google Platforms Rant," "Get That Job at Google"). When someone with that history says "this changes everything," it carries weight — but he's also been enthusiastic before. How do we calibrate?
- **The naming is doing work** — "Gas Town," "Beads," "Mayor," "Colony" — Yegge has deliberately built a vocabulary for this new way of working. Naming things is how paradigms solidify.
- **The [Steve Klabnik post](https://steveklabnik.com/writing/how-to-think-about-gas-town/)** — "How to Think About Gas Town" offers a useful outside perspective on what Yegge is really building and what it means for the broader ecosystem.
- **The [DoltHub day-in-the-life post](https://www.dolthub.com/blog/2026-01-15-a-day-in-gas-town/)** — A concrete, practical account of what actually happens when you spend a day in Gas Town. Great for grounding the abstract concepts.
- **TokenMaxxing as a metaphor** — The episode title is sarcastic, but it captures something real: a lot of current practice is about gaming context windows, prompt engineering around limits, and trying to extract maximum work from each session. It's a sign of immaturity in the tooling.

---

## Resources & Further Reading

- [Welcome to Gas Town](https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04) — Steve Yegge's original essay
- [The Future of Coding Agents](https://steve-yegge.medium.com/the-future-of-coding-agents-e9451a84207c) — Yegge's follow-up
- [Gas Town on GitHub](https://github.com/steveyegge/gastown) — The actual tool
- [How to Think About Gas Town](https://steveklabnik.com/writing/how-to-think-about-gas-town/) — Steve Klabnik's analysis
- [A Day in Gas Town](https://www.dolthub.com/blog/2026-01-15-a-day-in-gas-town/) — DoltHub's practical walkthrough
- [SWE-agent paper (arXiv)](https://arxiv.org/abs/2405.15793) — Princeton/Stanford, NeurIPS 2024
- [SWE-bench paper (arXiv)](https://arxiv.org/abs/2310.06770) — The benchmark that defined the field
- [Introducing Devin](https://cognition.ai/blog/introducing-devin) — Cognition Labs, March 2024
- [Gas Town, Beads, and the Rise of Agentic Development](https://softwareengineeringdaily.com/2026/02/12/gas-town-beads-and-the-rise-of-agentic-development-with-steve-yegge/) — Software Engineering Daily interview with Yegge
- [Top Coding Agents 2025](https://benched.ai/guides/top-coding-agents-2025) — Benched.ai comparison guide
