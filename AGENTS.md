# AGENTS.md

Instructions for AI coding agents operating in this repository.

## Project Overview

Content repository for the tokidev brand. All content is
written in Markdown. No build system or application code.

Content types:
- **Articles** — long-form technical blog posts
- **Instagram** — post captions, copy, and hashtag sets
- **Video** — scripts for YouTube videos and Reels
- **Brand** — guidelines, voice documentation, and assets

## Repository Structure

```
articles/
  published/    - Published articles
  drafts/       - Work-in-progress articles
  plans/        - Article plans and outlines
  templates/    - Reusable article templates

instagram/
  published/    - Approved and posted content
  drafts/       - Work-in-progress captions and copy
  plans/        - Campaign and content plans

video/
  published/    - Final approved scripts
  drafts/       - Work-in-progress scripts
  plans/        - Video plans and outlines

brand/
  guidelines/   - Voice, tone, and style documentation
  assets/       - Brand reference files

investigation/ - Shared research across all content types
```

## General Conventions

### Filenames

Kebab-case slugs. No dates or prefixes.
Examples: `building-cli-tools-in-go.md`, `launch-day-caption.md`

### Frontmatter

Every content file must start with frontmatter. The required
fields vary by content type (see below).

### Style

- Clear, conversational tone. Active voice. Short paragraphs.
- Wrap prose at **80 characters** (exceptions: URLs, code
  blocks, frontmatter values).
- Sentence case headings: "Setting up your environment".
- Use `-` for unordered lists, `1.` for ordered lists.
- No emoji unless the user requests it.
- No AI disclaimers, no marketing fluff, no fabricated data.

---

## Articles

### Frontmatter

```yaml
---
title: "Building CLI Tools in Go"
slug: building-cli-tools-in-go
date: 2026-03-15
author: t0k1dev
tags: [go, cli, tutorial]
status: draft              # draft | published
description: >
  A one-to-two sentence summary for previews and SEO.
---
```

### Body

- Introduction right after frontmatter (no heading).
- `##` for sections, `###` for subsections. Never skip levels.
- Code blocks must specify the language (` ```go `, ` ```bash `).
- End with a conclusion or wrap-up section.

### Tags

Use lowercase. Prefer existing tags. Common tags: `go`,
`rust`, `typescript`, `python`, `cli`, `devtools`,
`tutorial`, `guide`, `opinion`, `testing`, `docker`, `git`,
`web`, `api`, `architecture`, `ai`, `career`.

### Workflow

1. Create `articles/drafts/<slug>.md` with complete
   frontmatter and `status: draft`.
2. To publish: move to `articles/published/` and set
   `status: published`.

---

## Instagram

### Frontmatter

```yaml
---
title: "Short description of the post"
slug: post-slug
date: 2026-03-15
author: t0k1dev
tags: [ai, devtools]
status: draft              # draft | published
format: single             # single | carousel | reel
---
```

### Body

- Caption first — the text that appears in the post.
- Separate caption from hashtags with a blank line.
- Hashtags as a single block at the end.
- Keep captions under 2,200 characters.
- First line of the caption is the hook — make it work
  without "more".

### Workflow

1. Create `instagram/drafts/<slug>.md`.
2. To publish: move to `instagram/published/`.

---

## Video

### Frontmatter

```yaml
---
title: "Video title"
slug: video-slug
date: 2026-03-15
author: t0k1dev
tags: [ai, tutorial]
status: draft              # draft | published
format: youtube            # youtube | reel | short
duration: ~10min
---
```

### Body

- Hook (first 30 seconds) at the top — clearly marked.
- Use `[VISUAL:]`, `[CUT:]`, `[B-ROLL:]` for production
  notes inline.
- Sections marked with `##`.
- End with a call to action.

### Workflow

1. Create `video/drafts/<slug>.md`.
2. To publish: move to `video/published/`.

---

## Brand

- `brand/guidelines/` — voice, tone, style rules, and
  any documentation about the tokidev persona.
- `brand/assets/` — reference files, color values,
  approved taglines, bio copy.

No strict template. Files should be clearly named and
self-explanatory.

---

## Investigation

`investigation/` is shared across all content types.
Research that informs multiple formats or long-term
strategy lives here. No specific template — use whatever
structure fits the research.

---

## Git

- Imperative commit messages: "Add article on Go CLI tools".
- One piece of content per commit when possible.
- When restructuring or adding brand assets, describe the
  scope: "Add brand voice guidelines".
