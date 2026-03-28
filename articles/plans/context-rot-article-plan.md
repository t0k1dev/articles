# Article Plan: Context Rot -- Why AI Gets Worse the
# More You Talk to It

## Target audience

Developers who use AI tools daily. People who have felt
something go wrong in a long AI session but couldn't name
it. No ML background needed, but the reader writes code
and ships things.

## Thesis

The longer your conversation with an AI, the dumber it
gets. I've watched it happen in my own work -- the model
that's brilliant at turn 3 is a liability by turn 30.
This isn't a bug in one chatbot. It's a structural problem
baked into how every model works right now. Once you see
it, you can't unsee it. And you start building differently.

---

## Narrative arc

The article follows one thread: **I noticed something
breaking, I went looking for answers, and what I found
changed how I work.**

It moves from personal frustration -> naming the problem
-> recognizing it everywhere -> understanding why it
happens -> what I actually changed. The reader should feel
like they're following someone's investigation, not
reading a report.

---

## Tone

Personal, direct, and interesting. This is an opinion
piece written by a developer for developers -- not a
report, not a thought-leadership post, not a tutorial.

The voice should feel like a smart colleague talking to
you after a long day. They have seen something, thought
about it a lot, and now they are telling you what they
actually think -- not what sounds safe to say publicly.
Opinionated without being preachy. Honest without being
bleak.

Rules for the voice:
- Write in first person where it strengthens the point.
  "I spent an afternoon on something that used to take
  my team a week" lands harder than "developers report
  that..."
- Use short sentences when making a sharp point. Let them
  breathe.
- Data is supporting evidence, not the argument. Mention
  the finding in plain language; save the methodology
  (sample sizes, p-values, DOIs) for the references
  section. The reader is a developer, not a peer reviewer.
- No passive voice for important claims. "The job changed"
  not "the job has been changed by AI tools."
- Avoid: "it is worth noting", "one might argue",
  "in today's rapidly evolving landscape", "leverage"
  (the business word, not the mechanical one), "empower",
  "paradigm shift."
- If a sentence could appear in a McKinsey slide, rewrite
  it.

---

## Structure

### 1. HOOK -- The session that went sideways

**Goal:** Start with a single moment the reader has lived.
One opening, not two.

- Open in first person. A long coding session with an AI
  assistant. Things going great at first -- the model
  nailing my constraints, writing clean code, following
  the patterns I set. Then somewhere around turn 20 it
  starts forgetting decisions it made earlier. It
  introduces a bug in code it wrote 15 messages ago. It
  ignores a constraint I set at the top.
- The moment: I haven't changed. The model got worse.
- I didn't have a name for it until I found the Chroma
  study. They asked models to copy the word "apple" 5,000
  times. One started babbling gibberish. Another said
  *"I need to chill out."* Another just refused. These
  are billion-dollar models failing at copy-paste.
- The community started calling this "context rot" in
  2025. The name stuck because the experience is
  universal.

**Narrative beat:** Something is wrong. I'm not imagining
it.

---

### 2. DEFINE -- What's actually rotting

**Goal:** Give the reader a mental model they'll carry
through the rest of the article and beyond it.

- I think of it like a whiteboard. Every message you send
  gets written on it. The AI reads the whole thing every
  time it responds. Context rot is what happens when the
  board gets so crowded the model can still *see*
  everything but stops *paying attention* to most of it.
- This distinction matters: it's not running out of space.
  The quality of attention gets thinner with every message
  you add. The window isn't full. The focus is gone.
- *"A 1M-token window still rots at 50K tokens. The
  problem is noise, not capacity."*
- That hit me when I first read it. The bottleneck isn't
  how much the model can hold. It's how much it can
  actually use.

**Narrative beat:** Now I have a name for it. And I'm
starting to see it everywhere.

---

### 3. SYMPTOMS -- How rot shows up in real work

**Goal:** Make the reader nod before they see any data.
This section earns the right to present evidence next --
the reader needs to feel the problem in their own
experience first.

- **It forgets your rules.** You said "always use
  TypeScript interfaces, never type aliases" 20 messages
  ago. It just wrote a type alias.
- **It contradicts itself.** It picked approach A earlier.
  Now it's rewriting everything with approach B. No
  explanation. No awareness.
- **It starts making things up.** The longer the session,
  the more confidently wrong it gets. Early answers cite
  real APIs. Late answers invent them.
- **It loses the plot.** You asked it to refactor a
  module. Thirty messages in, it's solving a different
  problem entirely.
- **The code gets quietly worse.** The function it wrote
  at turn 5 was clean. The one at turn 25 contradicts
  the patterns it established earlier.
- **Instructions stop sticking.** This one's subtle.
  Accumulated context can actually *override* your system
  prompt. The model's behavior drifts toward whatever the
  bulk of the conversation looks like, not what you told
  it to do at the top. Anthropic's research on in-context
  learning shows this follows a power law -- the more
  stuff in the context, the stronger the override.

I've started treating it like a signal. When the model
feels "off," I don't push through. I check the context
length. Nine times out of ten, that's the answer.

**Narrative beat:** You've seen this. Now let me show you
the numbers.

---

### 4. EVIDENCE -- I went looking. It's worse than I
   thought.

**Goal:** Present the key research as discoveries in a
personal investigation. Three findings, not four
sub-studies. Each one lands a single point. Data supports
the story -- it doesn't replace it.

#### The middle is a dead zone

Stanford researchers found a U-shaped pattern: AI pays
the most attention to the top and bottom of the context.
Everything in the middle drops up to 30%.

That explained a lot. My system prompt sits at the top.
My latest message is at the bottom. But the architecture
decisions, the constraints we agreed on 15 turns ago --
those sit right in the dead zone. It's like reading a
long email thread and only remembering the first message
and the last reply.

#### The benchmarks are flattering lies

Standard "needle in a haystack" tests -- hide a fact,
ask the model to find it -- look great. Models score
90%+. But Adobe ran a version that required actual
understanding instead of keyword matching. Everything
collapsed:

- GPT-4o: 99% down to 70%
- Claude 3.5: 88% down to 30%
- Gemini 2.5 Flash: 94% down to 48%
- Llama 4: 82% down to 22%

The tests that make AI look good are the wrong tests.
Real work requires reasoning, not ctrl+F. And half the
models claiming 32K+ context couldn't even maintain
performance at that size on anything beyond trivial
retrieval (RULER benchmark, 2024).

#### Less context beats more context

This one changed how I build things. Chroma tested 18
major models and the finding that stuck hardest: giving
a model 300 tokens of relevant information dramatically
outperformed giving it 113,000 tokens of full
conversation history. Less was not just better. It was
*dramatically* better.

Other findings from the same study: one misleading
sentence in the context is enough to throw models off.
Well-organized text is actually harder for models to
search than random noise (still can't fully explain that
one). And when unsure, some models admit it (Claude)
while others confidently fabricate (GPT). Both are
rotting -- they just rot differently.

Brief timeline to anchor how recent this all is:
- 2023: Stanford publishes "Lost in the Middle"
- 2024: RULER proves advertised context sizes don't hold
- Feb 2025: Adobe's NoLiMa destroys the benchmark
  illusion
- Jun 2025: "context rot" coined on Hacker News
- Jul 2025: Chroma's 18-model study goes viral
- Sep 2025: Anthropic adopts the term, proposes "context
  engineering" as a discipline

**Narrative beat:** It's not in my head. Every model,
every provider, every benchmark that tests real work
confirms it.

---

### 5. WHY -- What's happening underneath (and why bigger
   windows don't fix it)

**Goal:** Explain just enough mechanics to satisfy
curiosity, then use that understanding to dismantle the
"bigger window" narrative. Two ideas in one section,
connected by cause and effect. Two minutes of reading,
max.

- Here's how I think about it. Every time the model reads
  your context, it decides how much attention to give each
  piece. That's a fixed budget. Add more messages, each
  one gets a thinner slice. Five people at dinner, you
  track every conversation. Fifty people, you catch
  fragments.
- On top of that, there's a structural bias. First tokens
  act as "attention sinks" -- they absorb focus regardless
  of what they say (Xiao et al. showed this at ICLR
  2024). Recent tokens benefit from recency. Everything
  in the middle gets starved. Not because it's less
  important. Because of where it sits.
- And most models trained on texts shorter than their max
  window. Push them to the edge and they're in unfamiliar
  territory.
- This is why bigger context windows don't solve it.
  Every few months a new model drops: 128K, 1M, 10M
  tokens. The implication is more memory, better AI.
  I bought into this for a while.
- But attention scales quadratically. Double the context,
  quadruple the relationships the model tracks. Something
  has to give. A bigger whiteboard doesn't help if you
  can only focus on one corner at a time.
- The numbers confirm it. On Reddit, local model users
  report usable context of 10K tokens on models that
  advertise 128K. That's less than 8%.
- Fair pushback: some people argue "context rot" is just
  a fancy name for something we already understood.
  Maybe. But "your database is slow" and "you have no
  index" describe the same problem at different levels of
  usefulness. The name helps people build around it.

**Narrative beat:** The ceiling is structural. Nobody's
fixing it next quarter. Now what?

---

### 6. WHAT I CHANGED

**Goal:** Share what actually changed in my workflow.
First person. Specific. No "best practices" energy.
Split by effort level so the reader can act immediately
on the first half.

#### Things I changed immediately

- **I start fresh more often.** When a session drifts, I
  don't fight it. I summarize where I am, open a new
  chat, paste the summary, keep going. It feels wasteful.
  It's not.
- **Important instructions go at the top.** Always. And I
  re-state them every 10-15 messages. The model doesn't
  get annoyed. It needs the reminder.
- **One problem per session.** I used to throw everything
  at one thread. Now I keep it focused. Database work in
  one session. Frontend in another. The quality difference
  is obvious.
- **I watch the token count.** Not obsessively. But I
  notice when things pass the halfway mark. That's when
  I get more careful about what I ask and what I keep in
  context.

#### Things I changed as a builder

- **Summarize, don't accumulate.** For tools I build, I
  compress old conversation history into summaries. Raw
  history is a context rot accelerator.
- **Retrieve, don't stuff.** Instead of cramming
  everything into the prompt, I pull in relevant context
  on demand. The Chroma finding proved this: 300 focused
  tokens beat 113K tokens of everything. Build for that.
- **Sub-tasks get their own context.** I break complex
  work into focused sub-tasks, each with a clean context.
  The results come back summarized. The parent context
  stays lean.
- **Notes live outside the chat.** The AI writes important
  findings to a file. When it needs that information
  later, it reads the file instead of searching 50
  messages of history. Anthropic calls this pattern
  "structured note-taking." I call it the only sane way
  to run a long session.

**Narrative beat:** This isn't theoretical. These changes
made my daily work noticeably better.

---

### 7. CLOSE -- The attention budget

**Goal:** Land one idea the reader remembers next time
they open a chat. Fold the forward-looking perspective
into the close instead of giving it a separate section --
the article should end on a concrete thought, not a
forecast.

- AI doesn't have memory. It has an attention budget.
  Every message costs a little of that budget. Context
  rot is what happens when you overdraw.
- This problem isn't getting fixed soon. It's
  architectural. But it's getting *managed*. Anthropic
  calls the discipline "context engineering." The best
  AI tools won't be the ones with the biggest windows.
  They'll be the ones that manage context so well you
  never notice the rot.
- Right now, most tools just let the window fill up and
  hope for the best. That's going to change. A year from
  now, context management will be table stakes.
- Until then: next time a session feels off -- the model
  forgetting things, contradicting itself, getting
  confidently wrong -- check how long the conversation
  is. That's probably your answer.
- Start fresh. It's the cheapest fix in AI.

---

## Suggested visuals

1. **The U-shaped curve** -- where AI actually pays
   attention in a long conversation (high at start and
   end, dead zone in the middle)
2. **The accuracy cliff** -- Adobe's numbers showing
   benchmark scores vs. real performance side by side
3. **The whiteboard** -- clean vs. crowded, same info,
   different attention quality
4. **300 vs. 113K** -- visual comparison of the Chroma
   finding that less context dramatically outperforms more
5. **Timeline** -- from "Lost in the Middle" (2023) to
   mainstream adoption of "context rot" (2026)

---

## Key sources

| Source | What it showed |
|--------|---------------|
| Lost in the Middle (Stanford, 2023) | The U-shaped attention pattern -- middle gets ignored |
| Attention Sinks (Xiao et al., ICLR 2024) | First tokens absorb attention regardless of content |
| Many-shot Jailbreaking (Anthropic, 2024) | Accumulated context overrides instructions via power law |
| RULER Benchmark (Hsieh et al., 2024) | Advertised context sizes don't hold on real tasks |
| NoLiMa (Adobe, 2025) | Standard benchmarks are misleading; semantic tasks fail much harder |
| Chroma Context Rot Study (2025) | 18 models tested, all degrade -- the most comprehensive look |
| Anthropic Engineering Blog (2025) | Named "context engineering"; proposed mitigation patterns |
| Understanding AI (Timothy B. Lee, 2025) | Clearest plain-language explanation of the scaling wall |

---

## Estimated length

~2,500-3,000 words (10-12 minute read).

## What to avoid

- Lit-review energy. Every study mention should feel like
  a personal discovery, not a citation.
- Passive voice on any main claim.
- The word "leverage" (the business one).
- Any sentence that could appear in a consulting deck.
- Hedging language: "it is worth noting", "one might
  argue", "in today's rapidly evolving landscape."
- Making the evidence section longer than the practical
  sections. The reader came for what to do, not what to
  read.
