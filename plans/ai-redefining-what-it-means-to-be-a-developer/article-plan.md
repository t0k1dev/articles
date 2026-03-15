# Plan: AI Is Redefining What It Means to Be a Developer

## Goal

Write a compelling opinion article that makes developers stop
and reconsider what their job actually looks like now that AI
tools are part of the workflow. Not hype, not fear — an honest
assessment.

## Target reader

Mid-level to senior developers who use AI tools casually but
have not fully thought through what the shift means for their
career and daily work.

## Core argument

The developer job title stayed the same but the actual work
changed. The value moved from writing code to directing it.
Developers who recognize this early will compound their
advantage.

## Tone

Personal, direct, and interesting. This is an opinion piece
written by a developer for developers — not a report, not a
thought-leadership post, not a tutorial.

The voice should feel like a smart colleague talking to you
after a long day. They have seen something, thought about it
a lot, and now they are telling you what they actually think
— not what sounds safe to say publicly. Opinionated without
being preachy. Honest without being bleak.

Rules for the voice:
- Write in first person where it strengthens the point.
  "I spent an afternoon on something that used to take my
  team a week" lands harder than "developers report that..."
- Use short sentences when making a sharp point. Let them
  breathe.
- Data is supporting evidence, not the argument. Mention
  the finding in plain language; save the methodology
  (sample sizes, p-values, DOIs) for the references section.
  The reader is a developer, not a peer reviewer.
- No passive voice for important claims. "The job changed"
  not "the job has been changed by AI tools."
- Avoid: "it is worth noting", "one might argue",
  "in today's rapidly evolving landscape", "leverage"
  (the business word, not the mechanical one), "empower",
  "paradigm shift."
- If a sentence could appear in a McKinsey slide, rewrite it.

---

## Impact phrases

Every section should contain at least one sentence that
could stand alone — something a reader would highlight,
quote, or remember after closing the tab. These are not
slogans. They are the moment where the argument crystallizes
into a single line.

Rules for impact phrases:
- Short. One sentence, two at most. If it needs a
  subordinate clause to make sense, it is not ready.
- Specific. "The job changed" is better than "the industry
  is undergoing a transformation." Concrete beats abstract.
- No setup required. The phrase should hit even if someone
  reads it out of context — in a tweet, a pull quote, a
  Slack message.
- Earned. Place them after the argument has been made, not
  before. The phrase lands when the reader already feels it
  is true and you give them the words for it.
- One per section maximum. More than one and none of them
  land.

Candidate phrases to work into the article. These are
starting points — improve, discard, or replace them as
the draft develops. Every section should end up with one:

- Introduction:
  "The job title is the same. The job is not."

- Section 2 (historical shift):
  "That is not an incremental improvement. That is a
  different game."

- Section 3 (bottleneck):
  "AI did not speed up the work. It made visible which
  part of the work was always the hard part."

- Section 4 (senior developer):
  "A senior developer's most valuable output is no longer
  a pull request. It is a decision."

- Section 5 (outship a team):
  "The leverage was always there. The tool just removed
  the team you needed to use it."

- Section 6 (codebase as context):
  "You are not just writing code for the next developer
  who reads it. You are writing it for the agent that
  will work in it."

- Section 7 (uncomfortable part):
  "Most of the code most of us wrote was assembly of
  known parts. AI just does that faster now."

- Section 8 (skills):
  "The skills that made you good at this job are not
  going away. They are just finally the whole job."

- Section 9 (closing):
  "The developer who shipped in an afternoon did not get
  lucky. They just noticed what changed."

---

## Sections

### 1. Introduction (no heading)

- Open in first person with something the writer actually
  did or witnessed — not a hypothetical. Something specific:
  a feature shipped in an afternoon that used to take days,
  a moment of realizing the old mental model no longer fit,
  a conversation with a colleague who hadn't noticed yet.
  The more specific and personal, the better. Generic scene-
  setting ("imagine a developer...") will lose the reader.
- Second paragraph: land the central observation. This is
  not a prediction about the future — it already happened.
  The job changed. The question is whether you noticed.
- Keep it to two short paragraphs. No thesis statement.
  Let the reader follow the thought, not receive a summary.

### 2. The biggest shift since open source

- This section gives the reader historical permission to
  take the claim seriously. It has happened before. Use
  that to lower their defenses, not to lecture them.
- The useful parallel to open source is *leverage*: a
  capability that used to require resources or headcount
  became available to anyone overnight. AI does the same
  thing to implementation speed.
- Cloud is the better parallel for job change: sysadmin and
  on-prem DBA roles shrank; DevOps and SRE appeared. The
  workforce reoriented, it did not disappear. Use this to
  defuse the "AI will take all jobs" anxiety without
  dismissing it.
- **Key line:** "That is not an incremental improvement.
  That is a different game."
- Keep the historical references brief and conversational.
  One sentence each. This is not a history lesson — it is
  context before moving to what actually matters.
- Do not cite Raymond. Cathedral/Bazaar is about governance
  models, not leverage. It muddies the argument.

### 3. Writing code is no longer the bottleneck

- The real insight here is slightly uncomfortable: the
  implementation work always hid how much of the hard work
  was actually thinking. AI removes the hiding. Name that.
- Use a specific task the writer has actually done with AI
  assistance — wiring an endpoint, writing a migration,
  scaffolding a component. Real experience beats a
  constructed example.
- The 55% faster stat (GitHub/Kalliamvakou, 2022) is useful
  here but introduce it conversationally: "GitHub ran a
  controlled study on this..." not as a citation block.
  Save the full reference for the references section.
- The Meyer et al. finding (developers spend a minority of
  their day writing code) can be a one-liner that reframes
  the section: if you were already spending most of your
  day not writing code, what exactly did AI accelerate?
- Land on the point clearly: the bottleneck moved. It is
  now upstream — in the decisions before the code.

### 4. What a senior developer actually does now

- This is the section the target reader is most likely to
  share or argue with. Make it land.
- Write it as an observation, not a prescription. "I've
  noticed that..." or "The role I actually play now is..."
  hits differently than "Senior developers must now..."
- The shift to articulate: less "how do I write this" and
  more "is this the right thing to write, and did AI get
  it right." The judgment moved upstream and downstream
  simultaneously — before the code (what to build) and
  after it (is this correct and safe).
- **Key line:** "A senior developer's most valuable output
  is no longer a pull request. It is a decision."
- The five new responsibilities are useful but should not
  read as a listicle. Introduce them in prose, then
  compress to a list only if it genuinely aids clarity.
  Five items:
  1. Writing precise problem descriptions for AI
  2. Evaluating output for correctness and security
  3. Setting architecture constraints before generation
  4. Knowing when to override and why
  5. Documenting decisions so future AI context is accurate

### 5. One developer can now outship a team

- This is where the article earns its most concrete moment.
  Use a real example if the writer has one. A project, a
  feature, a side thing that shipped. Not "developers can
  now..." — "I built X in Y" or "a friend of mine shipped
  Z alone and it used to take a team."
- Solo founders and side project builders are the clearest
  illustration because they have full context and no
  handoff cost. Name the advantage specifically.
- The 92% adoption and 46% of code stats (GitHub, 2023)
  show this is not hypothetical — it is already how most
  developers work. Drop them in as a single sentence.
- Hard limits — state these plainly, not as disclaimers:
  - Legacy codebases: AI degrades without context. The
    older and messier the code, the less useful AI is.
  - Ambiguous requirements: AI does not resolve ambiguity,
    it amplifies it. Garbage in, garbage out — faster.
  - Domain gaps: you cannot verify output in a domain you
    do not understand. The speed advantage disappears.
  - Security: a 2022 study found a meaningful percentage
    of Copilot suggestions contained security flaws. The
    tool does not know it is wrong. You have to.

### 6. Your codebase is a conversation with an AI agent

- This section introduces an idea most developers have not
  explicitly thought about. It should feel like a small
  revelation, not a best-practices checklist.
- The core observation: AI reads your repo the way a new
  hire reads it on their first day — except it cannot ask
  questions. The quality of what it produces depends
  entirely on what it can find.
- "Messy project = messy AI output" is the right framing.
  Keep it that blunt.
- AGENTS.md, Cursor rules, and similar files now serve two
  audiences: the human reading them and the AI using them.
  That is a genuinely new thing to be true about a codebase.
- Do not claim teams are "already restructuring" without a
  concrete example. If one cannot be found, frame this as
  where things are heading, not where they are.
- End with something practical the reader can do today.

### 7. The uncomfortable part

- This is the section that separates the article from a
  productivity blog post. Do not pull the punch.
- The honest observation: most of the code most developers
  wrote for most of their careers was pattern repetition.
  Not all of it. Not the hard parts. But most of it.
  AI makes this impossible to ignore.
- Write this from a first-person place. "I spent years
  proud of code that, in retrospect, was mostly assembly
  of known parts." Something like that. The admission
  makes the reader trust the rest.
- The data backs it up without being the argument:
  - Stripe (2018): ~70% of developer time on maintenance
    and debt, not new features
  - Code clone studies: 20–25% of large codebases are
    literally duplicated segments
  - 87% of developers say AI saves mental effort on
    repetitive tasks — developers themselves confirm it
- The uncomfortable implication: career identity and
  compensation were largely built on the implementation
  layer. AI is devaluing that layer. Say it plainly.
- Do not use the "80% repetition" figure as a fact — it
  is unsourced. Use what the data actually shows instead.
- End with something that opens into the next section
  rather than closing down. The point is not "you are
  replaceable" — it is "the part that mattered was always
  the other 30%, and now you can spend more time there."

### 8. The skills that actually matter now

- Frame this as a reframe, not a to-do list. The reader
  already has most of these skills. The shift is in which
  ones matter most, not in starting from zero.
- Keep the voice grounded: "The skills that made you good
  at this job are not going away. They are just finally
  the whole job."
- Five skills, written as observations not instructions:
  1. System design — you have to know where the pieces
     go before AI can fill them in
  2. Critical code review — evaluating output you did not
     write is now the core of the job, not a side task
  3. Problem articulation — describing what you want
     precisely enough that AI produces something useful
     is a real skill that takes practice
  4. Debugging under uncertainty — AI bugs are often
     confident and wrong; tracing them requires deep reading
  5. Domain knowledge — the one input AI cannot generate;
     understanding the problem space is irreplaceable
- What is losing value: memorizing syntax, writing
  boilerplate, scaffolding standard patterns from scratch.
  Not worthless — just no longer a differentiator.
- Do not frame this as a warning. Frame it as an opening.

### 9. Where to go from here

- Rename from "Adapt or fall behind" — that is fear
  framing and contradicts the tone of the rest of the
  piece. This section should feel like a conversation
  ending, not a warning.
- The point is not "learn fast or die." The point is:
  the shift already happened, you are probably already
  doing parts of it, here is how to do it more
  deliberately.
- Five concrete things, written as practical suggestions
  not imperatives:
  1. Use one AI tool on real work every day for a month.
     Not demos. Not tutorials. Real problems with stakes.
  2. Practice writing precise problem descriptions. Treat
     it like writing a spec — the quality of the input
     determines the quality of the output.
  3. Read more code than you write. Review and evaluation
     are now the core skill. Build that muscle.
  4. Study system design explicitly. Read architecture
     decision records from open source projects. Write
     your own.
  5. Document your projects as if an AI will read them —
     because one will. Clear naming, explicit conventions,
     a file that explains the rules.
- Close by returning to the personal image from the intro.
  The developer who shipped in an afternoon had already
  made these shifts. Not because they read an article —
  because they paid attention to what changed.

### 10. References

- End the article with a references section.
- Only cite sources that are actually used in the body.
  Do not pad the list.
- Format: author(s), title, publisher/venue, year, URL
  if stable. No footnote numbers — use inline callouts
  in the text (e.g., "a 2022 GitHub study found...") and
  list full citations here.
- Sources confirmed by the investigation to cite:

  1. Kalliamvakou, E. (2022). "Research: quantifying GitHub
     Copilot's impact on developer productivity and
     happiness." GitHub Blog, September 2022.
     https://github.blog/news-insights/research/research-
     quantifying-github-copilots-impact-on-developer-
     productivity-and-happiness/

  2. Meyer, A. N., Barton, L. E., Murphy, G. C.,
     Zimmermann, T., & Fritz, T. (2017). "The Work Life of
     Developers: Activities, Switches and Perceived
     Productivity." IEEE Transactions on Software
     Engineering, 43(12), 1178–1193.
     DOI: 10.1109/TSE.2017.2656886

  3. Kapser, C. J., & Godfrey, M. W. (2008). "'Cloning
     Considered Harmful' Considered Harmful: Patterns of
     Cloning in Software." Empirical Software Engineering,
     13(6), 645–692.
     DOI: 10.1007/s10664-007-9064-x

  4. Stripe. (2018). "The Developer Coefficient." Harris
     Poll, n=~11,000 developers and business leaders.
     (Original page no longer resolves; findings widely
     reported in 2018 tech press.)

  5. GitHub. (2023). "Octoverse: The state of open source
     and AI." GitHub, Inc.
     https://github.blog/news-insights/octoverse/octoverse-
     2023/

  6. Pearce, H., Ahmad, B., Tan, B., Dolan-Gavitt, B., &
     Karri, R. (2022). "Asleep at the Keyboard? Assessing
     the Security of GitHub Copilot's Code Contributions."
     IEEE Symposium on Security and Privacy (S&P 2022).
     (Stanford/NYU security study on Copilot vulnerabilities.)

  7. Forsgren, N., Storey, M. A., Maddila, C., Zimmermann,
     T., Houck, B., & Butler, J. (2021). "The SPACE of
     Developer Productivity." ACM Queue, 19(1), 20–48.
     DOI: 10.1145/3454124

- Sources to verify before publishing (found in
  investigation but need URL or DOI confirmation):
  - GitHub Enterprise Survey, June 2023, Wakefield Research,
    n=500 U.S. enterprise developers
  - JetBrains Developer Ecosystem Survey 2024, n=23,262

---

## Narrative arc

Introduction → reader is intrigued, not sure where this goes
Section 2 → historical context, this has happened before
Section 3 → the specific mechanics of what changed
Section 4 → what it means for the reader's daily work
Section 5 → the upside, with honest limits
Section 6 → a practical implication most readers haven't
            thought about
Section 7 → the hard truth (emotional low point)
Section 8 → reframe: the skills that matter are ones you
            have (emotional recovery)
Section 9 → concrete path forward, callback to intro —
            ends on possibility, not urgency

The arc should feel like: recognition → context → implication
→ honest discomfort → reframe → action.

---

## Writing checklist

- [ ] Frontmatter complete (title, slug, date, author, tags,
      status, description)
- [ ] Introduction opens with a first-person, specific moment
      — not a hypothetical or generic scene
- [ ] Each section has one clear point
- [ ] Every claim backed by evidence or marked as opinion
- [ ] Data cited conversationally in the body; methodology
      and DOIs only in the references section
- [ ] "80% repetition" not cited as a fact
- [ ] Limits and caveats are direct, not softened
- [ ] No passive voice for important claims
- [ ] None of: "leverage", "empower", "paradigm shift",
      "in today's landscape", "it is worth noting"
- [ ] No AI disclaimers or marketing language
- [ ] At least one moment of genuine first-person admission
      (something the writer actually experienced or thought)
- [ ] Section 9 title is not fear-framing
- [ ] Closing ties back to the opening image
- [ ] Lines wrapped at 80 characters
- [ ] Headings in sentence case
- [ ] References section present with full citations
- [ ] Every inline data point has a corresponding reference
- [ ] Each section contains one impact phrase that can
      stand alone out of context
- [ ] No section has more than one impact phrase
- [ ] Impact phrases are placed after the argument, not
      before it
