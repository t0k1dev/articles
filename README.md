# Articles

A collection of technical blog articles written in Markdown.

## Structure

```
articles/   - Published articles
drafts/     - Work-in-progress articles
templates/  - Article templates
```

## Writing a New Article

1. Create a file in `drafts/` using a kebab-case slug
   (e.g., `drafts/building-cli-tools-in-go.md`).
2. Add frontmatter at the top:

```yaml
---
title: "Building CLI Tools in Go"
slug: building-cli-tools-in-go
date: 2026-03-15
author: t0k1dev
tags: [go, cli, tutorial]
status: draft
description: >
  A short summary of the article.
---
```

3. Write the article body below the frontmatter.

## Publishing

Move the file from `drafts/` to `articles/` and set
`status: published` in the frontmatter.
