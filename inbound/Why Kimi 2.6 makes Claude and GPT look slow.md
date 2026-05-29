---
title: "Why Kimi 2.6 makes Claude and GPT look slow"
source: "https://x.com/defileo/article/2057118821614322126"
author:
  - "[[Defileo🔮 (@defileo)]]"
published: 2026-05-20
created: 2026-05-22
description: "Three weeks ago I wrote an intro to Kimi K2.6 and called it the model most people were sleeping on.The article landed, people tried it, half..."
tags:
  - "clippings"
---
![Immagine](https://pbs.twimg.com/media/HIn2rLwXAAAmSxN?format=jpg&name=large)

Three weeks ago I wrote an intro to Kimi K2.6 and called it the model most people were sleeping on.

The article landed, people tried it, half of them came back asking the same question.

"Okay, but how do I actually use this thing for real work?"

This is the answer, deeper than the intro, less surface, more tactics. The new features, the four modes most operators do not know exist, the prompts to copy and test today, and the use cases nobody is writing about yet.

If you read the first article, this is the follow-up you wanted, If you did not, you will catch up fast.

<video preload="none" tabindex="-1" playsinline="" aria-label="Video incorporato" poster="https://pbs.twimg.com/amplify_video_thumb/2056423630947352577/img/ORA-_1yk8pXZr8HC.jpg" style="width: 100%; height: 100%; position: absolute; background-color: black; top: 0%; left: 0%; transform: rotate(0deg) scale(1.005);"><source type="video/mp4" src="blob:https://x.com/3146773f-2cbf-4a85-8233-e9502ee4eb8c"></video>

![](https://pbs.twimg.com/amplify_video_thumb/2056423630947352577/img/ORA-_1yk8pXZr8HC.jpg?name=large)

If you haven't heard of [kimi.com](https://kimi.com/) 2.6 this video is the fast intro

## The quick refresher...

Kimi K2.6 is Moonshot AI's open source model, released April 20, 2026, it's free and around $0.55-0.80 per million input tokens via API, roughly 7-10x cheaper than Claude for the same work depending on output volume.

The technical headline is 300 sub-agents executing 4,000 coordinated steps in parallel. That is the agent swarm, one prompt -> hundreds of agents working simultaneously, one orchestrator merging the results.

That headline number is where most articles stop, the real story is why the architecture exists in the first place.

## Why Single-Agent AI has hit a structural ceiling

This is Moonshot's framing, not mine, and it lands harder than any tutorial.

For three years the AI industry has been refining the hammer. Faster inference, longer context, cheaper tokens. Every release has been about making the tool a little better.

The problem is the carpenter still has two hands and twenty-four hours in a day, a better hammer does not help if the bottleneck was never the hammer.

Here is the part most people skip, ask a single-agent deep research tool to survey a hundred companies or synthesize dozens of papers. As the task drags on, the context window fills up, the system falls back to history folding or summarization to make room for new tokens. That compression is lossy, and every subsequent reasoning step gets worse.

![Immagine](https://pbs.twimg.com/media/HIoBrIyWsAAmaT1?format=jpg&name=large)

This is not a bug or a temporary limitation. It is a structural ceiling imposed by the single-agent sequential execution model itself. You cannot fix it with a smarter model. You can only fix it by abandoning the architecture.

That is what Agent Swarm is, not a better single agent, but a reconstruction of the entire workshop.

<video preload="none" tabindex="-1" playsinline="" aria-label="Video incorporato" poster="https://pbs.twimg.com/amplify_video_thumb/2056458167219896320/img/_eGjqFOipOP1zbOh.jpg" style="width: 100%; height: 100%; position: absolute; background-color: black; top: 0%; left: 0%; transform: rotate(0deg) scale(1.005);"><source type="video/mp4" src="blob:https://x.com/33105659-7d39-41a7-9bbd-3eb61ec2f099"></video>

![](https://pbs.twimg.com/amplify_video_thumb/2056458167219896320/img/_eGjqFOipOP1zbOh.jpg?name=large)

K2.5 had 100 sub-agents and 1,500 coordinated steps. K2.6 has 300 sub-agents and 4,000 steps. Real-world results on long-horizon tasks deliver up to 4.5x faster execution than a sequential agent on the same work, with higher final quality because the swarm structurally avoids the context collapse that breaks single agents.

The headline numbers are real, and the reason they matter is that the bottleneck moved.

## Agent Swarm is an organization that designs itself

The line from Moonshot's research post that almost nobody quotes:

"This is not the story of many AI agents working together. What we are building is an organizational structure with bosses, employees, and divisions of labor, except this organization is not designed by humans. It designs itself."

When you give Agent Swarm a goal, you are not commanding an assistant. You are hiring a CEO. That CEO then finds the researchers, the analysts, the fact-checkers, all on its own.

You do not micromanage. You do not pick the team. You define the deliverable, and the swarm builds the organization needed to ship it.

## 🚨 Okay this is what Agent Swarm gave me as an answer to the simple question "Show me what you can do"

<video preload="none" tabindex="-1" playsinline="" aria-label="Video incorporato" poster="https://pbs.twimg.com/amplify_video_thumb/2056468744084271104/img/e4t1V4nTgUX3IyKj.jpg" style="width: 100%; height: 100%; position: absolute; background-color: black; top: 0%; left: 0%; transform: rotate(0deg) scale(1.005);"><source type="video/mp4" src="blob:https://x.com/14ec509d-02c1-4c86-90b2-bdf50d32b783"></video>

![](https://pbs.twimg.com/amplify_video_thumb/2056468744084271104/img/e4t1V4nTgUX3IyKj.jpg?name=large)

Literally in few minutes it send me a whole site where I can check how everything works.

That self-organization is the actual unlock. Every other "multi-agent" system on the market is LLM A calling LLM B in a fixed loop you had to design. Kimi's swarm builds the org chart from scratch every time, sized to the work in front of it.

## How the Swarm actually works

Five things happen under the hood when you submit a swarm task.

**Decomposition.** The coordinator breaks your goal into domain-specialized subtasks. Research goes to research agents, synthesis to synthesis agents, writing to writing agents.

**Agent matching.** Each subtask is routed to the sub-agent best suited based on skill and tools. This routing is why K2.6 hit 86.3% on BrowseComp in Swarm mode vs K2.5's 78.4%, same workers, smarter dispatch.

**Parallel execution.** All sub-agents work simultaneously with their own scoped context window, which is what kills the context-collapse problem that breaks single-agent runs.

**Failure recovery.** When a sub-agent stalls, the coordinator redirects and reassigns. The swarm self-heals during the run.

**Synthesis.** Outputs merge into one coherent deliverable with contradictions resolved.

There is a sixth thing nobody talks about: structural disagreement. Independent agents naturally arrive at different conclusions on overlapping questions, the coordinator forces reconciliation, and that structurally avoids groupthink. This is why swarm output often feels sharper than what one model produces.

Moonshot's own examples that prove it: the swarm pulled 200+ Paul Graham essays scattered across personal sites and archives into 6 topic-based folders with a full summary report, one prompt. Another run found the top 3 creators across 100 niche YouTube domains, defining each niche itself before dispatching 100 parallel sub-agents.

The pattern is the same in both: a mountain of things to find or process where each item is independent. That is the sweet spot. For sequential tasks where step N depends on step N-1, stay on single-agent mode.

How the swarm Actually Workss four. Instant for quick lookups, Thinking for analysis and complex code, Agent for medium autonomous tasks like a 10-page report, Agent Swarm only when work genuinely parallelizes. Most operators reach for Swarm by default and pay for parallelism they never use. Match mode to task size.

## Three underused features and what to build with them

**Run /plan before /swarm**, almost no one teaches this. /plan shows you exactly how Kimi will decompose your task into sub-agents and steps before any work happens. You see the plan, adjust if the agents are wrong, then commit.

```text
/plan [your goal with full context]
```

Costs nothing, a 200-agent swarm decomposed wrong costs real money.

**Document to Skills:** Upload your best work, a polished report, a landing page, a deck that closed a deal. Kimi captures the structural and stylistic fingerprint as a reusable skill that every future swarm applies automatically. Sitting in the menu, almost nobody uses it.

**Coding-driven design:** Same prompt, two different results. Claude defaults to clean templated layouts. Kimi treats UI as a coding problem first, paired with the MoonVIT encoder, and produces editorial layouts that feel intentionally composed. Prompt both with "design a landing page for The J Hotel" Claude returns a centered booking form on navy with gold accents, looks like every AI hotel page. Kimi returns a left-aligned editorial layout with a warm hero photo, "Book a Stay" floated over the image, typography that feels designed. If you ship front-end at scale, switch to Kimi for that part of the workflow.

<video preload="none" tabindex="-1" playsinline="" aria-label="Video incorporato" poster="https://pbs.twimg.com/amplify_video_thumb/2056481817683722240/img/no1YhmUV_DorKvVp.jpg" style="width: 100%; height: 100%; position: absolute; background-color: black; top: 0%; left: 0%; transform: rotate(0deg) scale(1.005);"><source type="video/mp4" src="blob:https://x.com/6a9297f4-98a9-44e9-addf-9f5447d66957"></video>

![](https://pbs.twimg.com/amplify_video_thumb/2056481817683722240/img/no1YhmUV_DorKvVp.jpg?name=large)

Comparison of Claude design and Kimi 2.6

**Six things to build today: >** Multi-phase market entry strategies producing PDF, Excel, and PowerPoint in one run. > Comparative academic deep dives pulling 24 months of related papers into a 40-page analysis. > Financial dashboards from raw CSVs with macro data integration. > Content library audits rewriting 50 old posts with consistent fingerprint. > Outreach at 300-prospect scale instead of 30 sequential. > Long-horizon code refactors splitting a 50,000-line legacy codebase by module, running autonomously over 24-36 hours.

## Three real prompts to test today:

These are operator-grade, scope locks, source rules, error handling, and threshold conditions, not the generic prompts that flood the timeline.

**Test 1: Agent Swarm parallel research**

Switch Kimi to Agent Swarm mode, then paste this.

```text
Scope: open-source AI models released between January 2026 and 
today. Coordinated swarm research, target 20 entries minimum.

For each entry, deliver:
- name, lab, release date
- parameter count (total + active if MoE)
- license type, commercial restrictions
- benchmark scores on SWE-bench Verified and BrowseComp 
  (primary sources only, no aggregators)
- one standout capability
- one known limitation

Source rules: official lab posts, papers, GitHub repos only. 
Flag any row where you cannot verify three independent sources.

If two sources conflict, flag the row and do not pick silently.

Output: single .xlsx with one row per model, plus a 200-word 
brief naming the three most important for an indie developer 
in 2026 with reasoning.

Stop and report if any sub-agent stalls for more than 10 minutes 
or if you cannot reach 15 verified entries.
```

What you should see: the swarm splitting research across multiple agents, each pulling from different sources in parallel, then merging into a single clean deliverable. Time it against doing this manually.

**Test 2: Document to Skills**

Find your best piece of professional work. A report, a proposal, a deck, anything you are proud of. Upload it and paste this.

```text
Capture this document as a reusable skill. Identify what makes 
it work:
- structure and section order
- tone and voice register  
- depth of analysis per section
- formatting decisions
- the writing rhythm and sentence cadence

Save the skill under the name "[whatever you want to call it]."

Then produce a brand new document on [different topic of your 
choice] using the captured skill. Match the original's quality 
bar without copying its specific content.

Stop and ask before publishing if any section deviates from the 
captured skill in tone or depth.
```

What you should see: a new document on a completely different topic that feels like the same author wrote it. This is the unlock for producing premium output at scale.

**Test 3: Plan mode for swarm validation**

Before any expensive swarm run, test the decomposition.

```text
/plan

Goal: build a complete competitive analysis of the AI coding 
agents market in Q2 2026. Deliverables:
- 30-page PDF report
- competitor feature matrix in Excel
- presentation deck with 20 slides
- recommendations summary for an indie developer choosing tools

Show me the proposed decomposition. How many sub-agents, what 
does each handle, estimated runtime, and where the biggest 
quality-drop risk sits.

Do not execute the swarm yet. Wait for confirmation.
```

What you should see: Kimi laying out exactly how it would attack the task before committing. Cheapest insurance policy you can buy before spinning up a 200-agent swarm.

## And one of the most important parts | The cost picture, honest.

A few rough numbers so you can calibrate:

Free tier on kimi gives you Instant and thinking modes immediately, agent and Agent Swarm require the Allegretto plan, tho straight I'd say it worth it.

API pricing sits around $0.55-0.80 per million input tokens and $2.65-3.60 per million output tokens depending on the endpoint and routing. Roughly 7-10x cheaper than Claude Opus for the same workload.

A 100-agent research run that produces a 40-page report with citations and a structured dataset usually runs $2-6 in tokens. Same work via Claude Code with manual orchestration costs $30-80 and takes three times longer.

Self-hosting is free if you have the hardware, weights are on Hugging Face under Modified MIT License.

**\- Leo**