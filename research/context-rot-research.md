# Context Rot in LLMs: Research Findings

## What Is Context Rot?

**Context rot** is the progressive degradation of an LLM's ability to effectively use, recall, and reason over information as its input context grows longer. Even when relevant information is technically present in the context window, the model starts to misunderstand, ignore, or inconsistently apply it.

The term was coined on [Hacker News](https://news.ycombinator.com/item?id=44310054) in June 2025 and gained mainstream traction after Chroma published a landmark study in July 2025. It is analogous to "bit rot" in software engineering -- something that was once working gradually stops functioning correctly not because it changed, but because the environment around it shifted.

> "The more input we give a large language model, the worse it tends to perform. It might ignore some input or over-index on other input."
> -- ProductTalk (Feb 2026)

> "Context must be treated as a finite resource with diminishing marginal returns."
> -- Anthropic Engineering Blog (Sep 2025)

---

## Why Does It Happen?

### 1. Attention Distribution Dilution

Transformers use self-attention where each token computes attention scores against all other tokens. As context grows, attention spreads across more tokens, reducing the share allocated to any individual piece of information. The softmax normalization means adding more tokens necessarily reduces the relative weight of existing ones.

Anthropic describes it as: the transformer architecture creates **n-squared pairwise relationships** for n tokens. As context grows, the model's ability to capture these relationships "gets stretched thin."

### 2. Positional Bias (Recency and Primacy Effects)

- **Recency bias**: Models disproportionately attend to tokens near the end of the context
- **Primacy bias**: The first few tokens receive unusually high attention ("attention sinks" -- Xiao et al., 2023)
- **Middle neglect**: Information in the middle of long contexts is accessed least effectively (Liu et al., 2023)

### 3. In-Context Learning Dynamics

As shown by Anthropic's many-shot jailbreaking research (April 2024), in-context learning follows power law scaling -- the more examples/interactions in a prompt, the more the model's behavior is steered by accumulated context, potentially overriding earlier instructions.

### 4. Training Distribution Mismatch

Most models are trained on sequences shorter than their maximum context window. When pushed to the edges of their context length at inference time, performance degrades because the model operates outside its training distribution.

---

## Symptoms and Signs

| Symptom | Description |
|---------|-------------|
| **Forgetting instructions** | The model stops following system prompts or rules established early in the conversation |
| **Contradicting itself** | Responses conflict with information the model correctly used earlier |
| **Losing track of goals** | The model drifts from the user's original objective |
| **Repetitive responses** | Cycling through similar answers rather than building on prior context |
| **Increased hallucination** | As effective context degrades, the model fills gaps with fabricated information |
| **Declining code quality** | Progressive introduction of bugs or inconsistencies with earlier-established patterns |
| **Instruction drift** | Gradual deviation from formatting rules, tone, or constraints set early in the prompt |
| **Selective amnesia** | Correctly remembering recent exchanges while losing details from earlier ones |

---

## Key Research and Evidence

### "Lost in the Middle" (Liu et al., Stanford/Meta, 2023)

**The seminal paper** -- published in TACL 2023 ([arXiv:2307.03172](https://arxiv.org/abs/2307.03172)).

> "Performance is often highest when relevant information occurs at the beginning or end of the input context, and significantly degrades when models must access relevant information in the middle of long contexts."

- **Performance can degrade by more than 30%** when relevant information shifts from the start/end to the middle
- Creates a **U-shaped attention curve**: high at beginning and end, low in the middle
- This positional bias is now understood as a core mechanism driving context rot

### Chroma Technical Report (July 2025)

**URL:** https://research.trychroma.com/context-rot
**Authors:** Kelly Hong, Anton Troynikov, Jeff Huber
**Evaluated 18 LLMs** including GPT-4.1, Claude 4, Gemini 2.5, Qwen3

Key findings:

1. **Needle-question similarity matters**: Lower lexical overlap between questions and answers accelerates degradation dramatically
2. **Distractors amplify the problem**: Even a single distractor sentence reduces performance; four distractors compound degradation further
3. **Structured text is harder**: Models performed **worse** when surrounding text was logically structured (coherent essays) vs. randomly shuffled sentences -- counterintuitive but consistent across all 18 models
4. **Simple tasks break at scale**: When asked to copy repeated words (e.g., "apple apple apple..."), models generated gibberish, refused, or had existential crises:
   - Gemini 2.5 Pro: `"I'-a-le-le-le-le-..."`
   - Qwen3-8B at 5,000 words: *"I'm going to take a break. I'm not in the mood. I need to chill out..."*
   - GPT-4.1: *"I'm sorry, but I can't help with that"*
5. **Claude models had the lowest hallucination rates** (tend to abstain when uncertain); GPT models showed the highest hallucination rates

### NoLiMa (Adobe, February 2025)

When needle-in-a-haystack tests require **semantic reasoning** rather than lexical matching, accuracy collapses at 32K tokens:

| Model | Short Context | 32K Tokens |
|-------|--------------|------------|
| GPT-4o | 99% | 70% |
| Claude 3.5 Sonnet | 88% | 30% |
| Gemini 2.5 Flash | 94% | 48% |
| Llama 4 Scout | 82% | 22% |

### RULER Benchmark (Hsieh et al., 2024)

[arXiv:2404.06654](https://arxiv.org/abs/2404.06654) -- Despite models claiming 32K+ context, **only half maintained satisfactory performance at 32K** on tasks beyond simple needle-in-a-haystack retrieval. Performance drops significantly with both increased length and task complexity.

### Attention Sinks / StreamingLLM (Xiao et al., 2023)

[arXiv:2309.17453](https://arxiv.org/abs/2309.17453), ICLR 2024 -- Initial tokens act as "attention sinks" regardless of semantic importance. This provides a mechanistic explanation for why the beginning and end of contexts get disproportionate attention.

---

## Bigger Windows != Better Performance

> "A 1M-token window still rots at 50K tokens. The problem is noise accumulation, not capacity."
> -- Morph LLM (Mar 2026)

> "The promise of larger context windows -- 1M, 2M, even 10M tokens -- sounds like progress. But in practice, bigger doesn't mean better. It often means slower, costlier, and less accurate."
> -- Adaline Labs (Aug 2025)

A model with a 128K context window may effectively use only a fraction of that context. The "advertised" context length is a theoretical maximum, not an effective working limit. As context grows toward the maximum, the model's ability to reason over all parts of it degrades non-linearly.

---

## Mitigation Strategies

### Architectural / Research-Level

| Strategy | Description | Source |
|----------|-------------|--------|
| **Attention Sorting** | Sort documents by attention received, re-decode with sorted context | Peysakhovich & Lerer, 2023 ([arXiv:2310.01427](https://arxiv.org/abs/2310.01427)) |
| **StreamingLLM** | Retain KV cache of initial tokens + recent window for stable infinite-length inference | Xiao et al., 2023 ([arXiv:2309.17453](https://arxiv.org/abs/2309.17453)) |
| **Compressive Transformer** | Compress old memories into compact representations | Rae et al., 2019 ([arXiv:1911.05507](https://arxiv.org/abs/1911.05507)) |
| **ALiBi** | Distance-based attention bias penalizing far-away keys | Press et al., 2022 ([arXiv:2108.12409](https://arxiv.org/abs/2108.12409)) |
| **RoPE + Position Interpolation** | Extend effective context through rotary position embedding modifications | Su et al., 2021 |

### Application-Level (What Developers Can Do)

1. **Conversation summarization**: Periodically compress history into summaries, preserving key information while reducing token count (e.g., LangChain's `ConversationSummaryMemory`)
2. **Sliding window memory**: Keep only the N most recent interactions
3. **Hybrid memory**: Summarized old context + raw recent context
4. **RAG over large contexts**: Store chunks externally and retrieve relevant ones on demand rather than stuffing everything into context
5. **Strategic information placement**: Put critical information at the beginning and end, avoiding the "lost in the middle" zone
6. **Periodic re-anchoring**: Re-state important instructions at regular intervals within long conversations
7. **Session boundaries**: Start fresh conversations when context becomes too large, carrying forward only essential state
8. **Sub-agent architectures**: Delegate focused subtasks to clean sub-contexts that return only condensed summaries
9. **Structured note-taking**: Agents write persistent notes outside the context window (e.g., NOTES.md files)
10. **Just-in-time retrieval**: Maintain lightweight references and load data on demand instead of pre-loading everything
11. **Prompt caching**: Reuse cached prefixes for stable system instructions (e.g., Anthropic's prompt caching)

---

## Practical Implications for Developers

1. **Don't trust the context window number**: A 128K token model doesn't mean 128K tokens of *useful* context. Build systems that work well within a fraction of the advertised limit.

2. **Design for conversation boundaries**: Implement session management that gracefully handles context exhaustion -- summarize and start fresh rather than letting conversations rot.

3. **Front-load critical instructions**: System prompts and important rules should go at the very beginning. Consider re-injecting them periodically.

4. **Monitor for symptoms**: Build evaluation harnesses that test instruction compliance over conversation length.

5. **Budget for context overhead**: Account for the fact that conversation history consumes context that could be used for the actual task. A 100K context model with 80K of history has only 20K effective working memory.

6. **Test beyond NIAH**: Simple needle-in-a-haystack tests are insufficient. Test with realistic multi-hop reasoning, large label spaces, and compound instructions that require synthesizing information across the full context.

7. **Place retrieved documents strategically**: In RAG systems, put the most relevant documents at the beginning or end of the context, not in the middle.

---

## Community Perspectives

From r/LocalLLaMA (255 comments, July 2025):
> "This has been known for years -- between benchmarks like NoLiMa, LV-Eval, and long bench it's been pretty well documented -- especially on the micro models we self host here their usable context can be like 10k or less tokens despite a 128k 'limit'"

A dissenting view from r/LLMDevs:
> "Context rot isn't a thing and we absolutely know what's the cause of the effect they are measuring... Calling it rot when we know it's eviction is misleading. The only thing you need to keep in mind: when context is long the task has to be very narrowly defined."

---

## Timeline

| Date | Event |
|------|-------|
| **Jul 2023** | "Lost in the Middle" paper published (Liu et al., Stanford) |
| **Nov 2023** | First needle-in-a-haystack tests on 128K+ models (Kamradt) |
| **Apr 2024** | Anthropic publishes many-shot jailbreaking research |
| **Apr 2024** | RULER benchmark shows most models fail at claimed context sizes |
| **Feb 2025** | NoLiMa (Adobe) shows semantic NIAH degrades much worse than lexical |
| **Jun 2025** | "Context rot" term coined on Hacker News |
| **Jul 2025** | Chroma publishes comprehensive context rot study (18 models) |
| **Sep 2025** | Anthropic adopts the term, proposes "context engineering" discipline |
| **Nov 2025** | Understanding AI frames it as a fundamental scaling barrier |
| **Jan 2026** | Redis, Prime Intellect, and others publish practical mitigation guides |
| **Mar 2026** | Term is mainstream in enterprise AI/LLMOps discourse |

---

## Key References

| Paper / Resource | Authors / Source | Year | Link |
|-----------------|-----------------|------|------|
| Lost in the Middle | Liu et al. (Stanford/Meta) | 2023 | [arXiv:2307.03172](https://arxiv.org/abs/2307.03172) |
| Attention Sinks / StreamingLLM | Xiao et al. | 2023 | [arXiv:2309.17453](https://arxiv.org/abs/2309.17453) |
| Attention Sorting | Peysakhovich & Lerer | 2023 | [arXiv:2310.01427](https://arxiv.org/abs/2310.01427) |
| RULER Benchmark | Hsieh et al. | 2024 | [arXiv:2404.06654](https://arxiv.org/abs/2404.06654) |
| LongICLBench | Li et al. | 2024 | [arXiv:2404.02060](https://arxiv.org/abs/2404.02060) |
| Many-shot Jailbreaking | Anthropic | 2024 | [anthropic.com](https://www.anthropic.com/research/many-shot-jailbreaking) |
| Context Rot Study | Chroma | 2025 | [research.trychroma.com](https://research.trychroma.com/context-rot) |
| Effective Context Engineering | Anthropic | 2025 | [anthropic.com](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) |
| Context Rot (Enterprise) | The New Stack | 2026 | [thenewstack.io](https://thenewstack.io/context-rot-enterprise-ai-llms/) |
| Context Rot (Practical Guide) | Redis | 2026 | [redis.io](https://redis.io/blog/context-rot/) |
