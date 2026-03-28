---
title: "Context Rot: Why AI Gets Worse the More You Talk to It"
slug: context-rot-why-ai-gets-worse-the-more-you-talk-to-it
date: 2026-03-28
author: t0k1dev
tags: [ai, architecture, opinion, devtools]
status: draft
description: >
  The longer your conversation with an AI, the worse it
  performs. An investigation into context rot and what it
  changed about how I work.
---

Last month I was deep into a refactoring session with an AI
assistant. The first twenty minutes were great. The model
understood my constraints, followed the patterns I set, wrote
clean code that fit the codebase. I was moving fast.

Then something shifted. Around turn 20, it introduced a bug
in a function it had written 15 messages earlier. I corrected
it. A few turns later, it ignored a naming convention I had
established at the top of the session. By turn 30 it was
recommending an architecture that contradicted decisions we
had made together an hour ago. No acknowledgment. No
awareness.

I hadn't changed. The model got worse.

I didn't have a name for it until I found the Chroma study.
Researchers asked models to do something a child could do:
copy the word "apple" five thousand times. Qwen said *"I'm
going to take a break. I'm not in the mood. I need to chill
out."* Gemini started babbling gibberish. GPT just refused.
These are billion-dollar models failing at copy-paste.

The community started calling this "context rot" in mid-2025.
The name stuck because the experience is universal. If you
have used an AI assistant for more than fifteen minutes, you
have felt it.

## What's actually rotting

I think of it like a whiteboard. Every message you send gets
written on it. The AI reads the entire board every time it
responds. Context rot is what happens when the board gets so
crowded that the model can still *see* everything but stops
*paying attention* to most of it.

This distinction matters. It's not running out of space. The
quality of attention gets thinner with every message you add.
The window isn't full. The focus is gone.

*"A 1M-token window still rots at 50K tokens. The problem is
noise, not capacity."*

That hit me when I first read it. The bottleneck isn't how
much the model can hold. It's how much it can actually use.

## How rot shows up in real work

You have seen this. Maybe you haven't named it, but you have
seen it.

**It forgets your rules.** You said "always use TypeScript
interfaces, never type aliases" twenty messages ago. It just
wrote a type alias.

**It contradicts itself.** It picked approach A earlier. Now
it's rewriting everything with approach B. No explanation. No
awareness that it changed course.

**It starts making things up.** The longer the session, the
more confidently wrong it gets. Early answers cite real APIs.
Late answers invent them.

**It loses the plot.** You asked it to refactor a module.
Thirty messages in, it's solving a different problem entirely.

**The code gets quietly worse.** The function it wrote at turn
5 was clean. The one at turn 25 contradicts the patterns it
established earlier. Same session, same model, different
quality.

**Instructions stop sticking.** This one is subtle.
Accumulated context can actually *override* your system
prompt. The model's behavior drifts toward whatever the bulk
of the conversation looks like, not what you told it to do at
the top. Anthropic's research on in-context learning shows
this follows a power law -- the more stuff in the context, the
stronger the override.

I have started treating it like a signal. When the model feels
"off," I don't push through. I check the context length. Nine
times out of ten, that's the answer.

## I went looking. The numbers are worse than I expected.

Once I had the name, I went looking for research. What I found
confirmed everything I had been feeling -- and then made it
worse.

### The middle is a dead zone

Stanford researchers found a U-shaped pattern: AI pays the
most attention to what's at the top and bottom of the context.
Everything in the middle drops by up to 30%.

That explained a lot. My system prompt sits at the top. My
latest message is at the bottom. But the architecture
decisions, the constraints we agreed on fifteen turns ago --
those sit right in the dead zone. It's like reading a long
email thread and only remembering the first message and the
last reply.

### The benchmarks are flattering lies

Standard "needle in a haystack" tests -- hide a fact in a long
text, ask the model to find it -- look great. Models score
above 90%. But Adobe ran a version that required actual
understanding instead of keyword matching. Everything
collapsed:

- GPT-4o: 99% down to 70%
- Claude 3.5: 88% down to 30%
- Gemini 2.5 Flash: 94% down to 48%
- Llama 4: 82% down to 22%

The tests that make AI look good are the wrong tests. Real
work requires reasoning, not ctrl+F. And half the models
claiming 32K+ context couldn't even maintain performance at
that size on anything beyond trivial retrieval.

### Less context beats more context

This one changed how I build things. Chroma tested 18 major
models and the finding that stuck hardest: giving a model 300
tokens of relevant information dramatically outperformed
giving it 113,000 tokens of full conversation history.

Less was not just better. It was *dramatically* better.

Other findings from the same study: one misleading sentence in
the context is enough to throw models off. Well-organized text
is actually harder for models to search than random noise -- I
still can't fully explain that one. And when unsure, some
models admit it (Claude) while others confidently fabricate
(GPT). Both are rotting. They just rot differently.

This entire body of research is recent. Stanford published
"Lost in the Middle" in 2023. The RULER benchmark proved
advertised context sizes don't hold in 2024. Adobe's NoLiMa
destroyed the benchmark illusion in early 2025. The term
"context rot" got coined on Hacker News in June 2025. Chroma's
18-model study went viral a month later. By September,
Anthropic adopted the term and proposed "context engineering"
as a discipline. The problem went from unnamed frustration to
mainstream understanding in about two years.

## Why it happens (and why bigger windows don't fix it)

Here's how I think about it. Every time the model reads your
context, it decides how much attention to give each piece.
That attention is a fixed budget. Add more messages, each one
gets a thinner slice. Five people at dinner, you can track
every conversation. Fifty people, you catch fragments.

On top of that, there's a structural bias. The first tokens in
a context act as "attention sinks" -- they absorb focus
regardless of what they actually say. Recent tokens benefit
from recency. Everything in the middle gets starved. Not
because it's less important. Because of where it sits.

Most models were also trained on texts shorter than their
maximum window. Push them to the edge and they're in
unfamiliar territory. They have never seen this much context
during training.

None of this is a bug someone will patch. It's how the
architecture works.

This is also why bigger context windows don't solve it. Every
few months a new model drops with a bigger number: 128K, 1M,
10M tokens. The implication is more memory, better AI. I
bought into this for a while.

But attention scales quadratically. Double the context,
quadruple the relationships the model has to track. Something
has to give. A bigger whiteboard doesn't help if you can only
focus on one corner at a time.

The numbers confirm it. On Reddit, local model users report
usable context of around 10K tokens on models that advertise
128K. That's less than 8% of what's on the box.

Fair pushback: some people argue "context rot" is just a fancy
name for something we already understood. Maybe. But "your
database is slow" and "you have no index" describe the same
problem at different levels of usefulness. The name helps
people build around it.

## What I changed

Once I understood the problem, I changed how I work. Some of
these are habits I adjusted overnight. Others took longer.

### Things I changed immediately

**I start fresh more often.** When a session drifts, I don't
fight it. I summarize where I am, open a new chat, paste the
summary, keep going. It feels wasteful. It's not.

**Important instructions go at the top.** Always. And I
re-state them every ten to fifteen messages. The model doesn't
get annoyed. It needs the reminder.

**One problem per session.** I used to throw everything at one
thread. Now I keep it focused. Database work in one session.
Frontend in another. The quality difference is obvious.

**I watch the token count.** Not obsessively. But I notice
when things pass the halfway mark. That's when I get more
careful about what I ask and what I keep in context.

### Things I changed as a builder

**Summarize, don't accumulate.** For tools I build, I compress
old conversation history into summaries. Raw history is a
context rot accelerator.

**Retrieve, don't stuff.** Instead of cramming everything into
the prompt, I pull in relevant context on demand. The Chroma
finding proved this: 300 focused tokens beat 113K tokens of
everything. Build for that.

**Sub-tasks get their own context.** I break complex work into
focused sub-tasks, each with a clean context. The results come
back summarized. The parent context stays lean.

**Notes live outside the chat.** The AI writes important
findings to a file. When it needs that information later, it
reads the file instead of searching through fifty messages of
conversation history. Anthropic calls this pattern "structured
note-taking." I call it the only sane way to run a long
session.

## The attention budget

AI doesn't have memory. It has an attention budget. Every
message you send costs a little of that budget. Context rot is
what happens when you overdraw.

This problem isn't getting fixed soon. It's architectural. But
it's getting *managed*. Anthropic calls the discipline
"context engineering." The best AI tools won't be the ones
with the biggest windows. They'll be the ones that manage
context so well you never notice the rot.

Right now, most tools just let the window fill up and hope for
the best. That's going to change. A year from now, context
management will be table stakes.

Until then: next time a session feels off -- the model
forgetting things, contradicting itself, getting confidently
wrong -- check how long the conversation is. That's probably
your answer.

Start fresh. It's the cheapest fix in AI.

---

## References

- Liu et al., "Lost in the Middle: How Language Models Use
  Long Contexts," Stanford/Meta, TACL 2023.
  [arXiv:2307.03172](https://arxiv.org/abs/2307.03172)
- Xiao et al., "Efficient Streaming Language Models with
  Attention Sinks," ICLR 2024.
  [arXiv:2309.17453](https://arxiv.org/abs/2309.17453)
- Anthropic, "Many-shot Jailbreaking," 2024.
  [anthropic.com](https://www.anthropic.com/research/many-shot-jailbreaking)
- Hsieh et al., "RULER: What's the Real Context Size of
  Your Long-Context Language Models?," COLM 2024.
  [arXiv:2404.06654](https://arxiv.org/abs/2404.06654)
- Adobe, "NoLiMa: Long-Context Evaluation Beyond Literal
  Matching," 2025.
- Hong, Troynikov, Huber, "Context Rot," Chroma, 2025.
  [research.trychroma.com](https://research.trychroma.com/context-rot)
- Anthropic, "Effective Context Engineering for AI Agents,"
  2025.
  [anthropic.com](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- Timothy B. Lee, "Context Rot: The Emerging Challenge,"
  Understanding AI, 2025.
