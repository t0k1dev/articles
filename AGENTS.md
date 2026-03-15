# AGENTS.md

Instructions for AI coding agents operating in this repository.

## Project Overview

Content repository for technical blog articles written in Markdown.
No build system or application code — just Markdown files.

## Repository Structure

```
articles/   - Published articles (one .md per article)
drafts/     - Work-in-progress articles
templates/  - Reusable article templates
```

## Article Conventions

### Filename

Kebab-case slug: `building-cli-tools-in-go.md`. No dates or prefixes.

### Frontmatter

Every article must start with:

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

## Style

- Clear, conversational tone. Active voice. Short paragraphs.
- Wrap prose at **80 characters** (exceptions: URLs, code blocks).
- Sentence case headings: "Setting up your environment".
- Use `-` for unordered lists, `1.` for ordered lists.
- No emoji unless the user requests it.
- No AI disclaimers, no marketing fluff, no fabricated data.

## Creating an Article

1. Pick a kebab-case slug from the topic.
2. Create `drafts/<slug>.md` (or `articles/` if publishing directly).
3. Add complete frontmatter with today's date and `status: draft`.
4. Write the body following the conventions above.

## Publishing a Draft

1. Move the file from `drafts/` to `articles/`.
2. Set `status: published` and update `date` in frontmatter.

## Git

- Imperative commit messages: "Add article on Go CLI tools".
- One article per commit when possible.

## Tags

Use lowercase. Prefer existing tags. Common tags: `go`, `rust`,
`typescript`, `python`, `cli`, `devtools`, `tutorial`, `guide`,
`opinion`, `testing`, `docker`, `git`, `web`, `api`, `architecture`.
