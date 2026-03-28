---
title: "Investigation Plan: Context Rot for AI Agents"
slug: context-rot-for-agents
date: 2026-03-28
author: t0k1dev
tags: [ai, agents, context, architecture, research]
status: draft
description: >
  Research plan to investigate context rot — the degradation
  of AI agent performance as conversation context grows,
  becomes stale, or accumulates contradictions. What causes
  it, how to measure it, and what practitioners are doing
  about it.
---

## Research question

What is context rot in AI agents, how does it manifest in
practice, and what are the current strategies to mitigate it?

---

## Why this matters

AI coding agents (Copilot, Cursor, OpenCode, Aider, Claude
Code) operate on a context window — a finite memory of
instructions, code, conversation history, and tool output.
As that context grows during a session, performance degrades
in ways that are poorly documented and not widely understood
by developers using these tools daily.

This is not a hypothetical problem. Anyone who has used an
agent for more than 30 minutes on a real project has seen
it: the agent starts repeating itself, forgets instructions
from earlier, contradicts its own decisions, or produces
output that ignores constraints it was given at the start.

Understanding this well enough to write about it — or to
build systems that resist it — requires actual research.

---

## Scope

### In scope
- Definition and taxonomy of context rot
- How context windows work across major LLMs (token limits,
  attention degradation, recency bias)
- Observable symptoms in coding agents specifically
- Measurement: how do you quantify context rot?
- Mitigation strategies currently in use (summarization,
  context pruning, checkpoint/resume, RAG, instruction
  pinning, sliding windows)
- Tool-specific approaches: how OpenCode, Cursor, Aider,
  Claude Code, and Copilot handle long sessions
- AGENTS.md / CLAUDE.md / Cursor rules as a mitigation
  (pinned context that survives rot)
- Practical advice for developers: when to start a new
  session, how to structure prompts for durability

### Out of scope
- Theoretical transformer architecture deep dive (mention
  attention mechanisms but don't explain the math)
- Fine-tuning or training-time solutions
- Non-coding agent use cases (customer support, etc.)
- Benchmarking LLMs against each other

---

## Research areas

### 1. Definition and taxonomy

**Goal:** Establish clear terminology.

**Questions:**
- What is context rot? Is there an established term or is
  this emerging vocabulary?
- Related terms: context drift, context pollution, context
  window overflow, instruction decay, attention dilution
- Is there a difference between context rot (gradual
  degradation) and context overflow (hard limit hit)?
- How do practitioners talk about this in forums, Twitter,
  Discord?

**Sources to check:**
- Academic papers on attention degradation in long contexts
- LLM documentation (OpenAI, Anthropic, Google) on context
  window behavior
- Developer blog posts and threads about agent failures in
  long sessions
- r/LocalLLaMA, r/ChatGPT, Hacker News threads
- AI engineering communities (Latent Space, AI Engineer)

---

### 2. How context windows actually work

**Goal:** Understand the mechanics well enough to explain
them to developers who don't read ML papers.

**Questions:**
- What happens when a context window fills up? Truncation
  vs. summarization vs. sliding window
- How does attention degrade with distance? (Lost in the
  middle problem)
- Does the position of information in the context affect
  how well the model uses it?
- What is the practical effective context length vs. the
  advertised maximum? (e.g. 200k tokens advertised but
  performance degrades after 20-30k)
- How do different models compare? Claude 200k vs GPT-4
  128k vs Gemini 1M — does more always help?

**Sources to check:**
- "Lost in the Middle" paper (Liu et al., 2023)
- Anthropic's research on long context performance
- OpenAI documentation on context handling
- Independent benchmarks (RULER, Needle in a Haystack)
- LMSys / Chatbot Arena long-context evaluations

---

### 3. Observable symptoms in coding agents

**Goal:** Catalog the specific ways context rot manifests
when using AI coding agents on real projects.

**Questions:**
- What are the most common failure modes?
  - Forgetting earlier instructions or constraints
  - Contradicting previous decisions
  - Repeating code it already generated
  - Ignoring AGENTS.md / project rules added at start
  - Generating code that conflicts with earlier output
  - Losing track of file state (editing stale versions)
  - Hallucinating file paths or function names from earlier
    in the conversation
- At what point in a session do these typically start?
- Does it correlate with token count, number of tool calls,
  number of files touched, or elapsed time?
- Are there agent-specific patterns? (e.g. does Cursor
  degrade differently than OpenCode?)

**Sources to check:**
- GitHub issues on Cursor, Aider, OpenCode, Claude Code
- Developer experience reports (blog posts, tweets, videos)
- Personal experimentation with controlled tasks
- Agent documentation on session management

---

### 4. Measurement and detection

**Goal:** Find out if anyone has proposed ways to measure
context rot, or define approaches ourselves.

**Questions:**
- Are there benchmarks that specifically test for context
  rot over long interactions?
- Can you detect rot programmatically? (e.g. ask the agent
  to repeat its original instructions at intervals)
- Is there a correlation between output quality metrics
  (correctness, consistency) and context length?
- What would a "context rot score" look like?

**Sources to check:**
- Academic benchmarks for long-context LLMs
- Software engineering research on AI-assisted coding
- Any open-source tools for monitoring agent performance
  over time

---

### 5. Mitigation strategies

**Goal:** Catalog every known approach to reducing or
managing context rot.

**Questions and areas:**
- **Context pruning:** removing old/irrelevant messages.
  Who does this? How aggressive?
- **Summarization:** compressing earlier conversation into
  summaries. Trade-offs? What gets lost?
- **Checkpoint/resume:** ending a session and starting
  fresh with a summary handoff. How do agents handle this?
- **RAG (Retrieval Augmented Generation):** pulling in
  context on demand instead of stuffing it all upfront.
  Effectiveness for code context?
- **Instruction pinning:** keeping system prompts and key
  instructions at the top of context regardless of length.
  How do different agents implement this?
- **Sliding windows:** dropping the oldest context as new
  context arrives. What gets lost?
- **Hierarchical context:** different priority levels for
  different types of context (system > project rules >
  recent code > old conversation)
- **AGENTS.md / CLAUDE.md / Cursor rules:** static
  instruction files that get injected every time. How
  effective is this as a rot mitigation?
- **Structured output:** using schemas and templates to
  constrain output even when context is degraded
- **Multi-agent architectures:** splitting work across
  agents with separate contexts to avoid single-context
  rot

**Sources to check:**
- Agent documentation and source code
- LangChain, LlamaIndex, CrewAI docs on memory management
- Research papers on retrieval-augmented generation
- Practical guides from AI engineering practitioners

---

### 6. Tool-specific approaches

**Goal:** Document how each major coding agent handles
long sessions and context management.

**Tools to investigate:**
- **OpenCode** — how does it manage context? Summarization?
  Pruning? Session handoff?
- **Cursor** — composer memory, .cursor/rules, how does it
  handle long sessions?
- **Aider** — repo map, chat history management, /clear
  command behavior
- **Claude Code** — CLAUDE.md, compact command, memory
  system, how it manages tool output
- **GitHub Copilot** — workspace context, how does the
  agent version handle extended tasks?
- **Windsurf / Codeium** — cascade memory, context
  management approach

**Sources to check:**
- Official documentation for each tool
- Source code (for open-source tools)
- Changelog / release notes mentioning context improvements

---

### 7. Practical developer guidance

**Goal:** Synthesize findings into actionable advice.

**Questions:**
- When should you start a new agent session?
- How should you structure AGENTS.md to survive context rot?
- What information should you front-load vs. provide
  on demand?
- How should you organize prompts for long tasks?
- Are there project structure patterns that resist rot?
  (e.g. small focused files vs. large monolithic ones)
- What habits separate developers who use agents well in
  long sessions from those who don't?

---

## Output format

This research should produce:
1. A consolidated findings document in `research/`
2. Enough material to inform at least one article, one
   carousel, and potentially a video script
3. A clear thesis or set of theses about context rot that
   can drive the content

---

## Research approach

1. Start with academic and technical sources (papers,
   documentation, benchmarks) to establish facts
2. Survey practitioner experience (blog posts, forums,
   issues) to ground the theory in real usage
3. If possible, run a simple controlled experiment:
   give an agent a task with specific constraints at the
   start of a session, then measure how well it follows
   those constraints after 10, 20, 30, 50 messages
4. Synthesize into a findings document with citations

---

## Open questions

- Is "context rot" the right term? Or is there an
  established term I should use instead?
- Is this primarily a model problem (attention architecture)
  or a tool problem (how agents manage the window)?
- Does context rot affect all LLMs equally or do some
  architectures handle it better?
- Is anyone building agents specifically designed to resist
  context rot? What does that architecture look like?
