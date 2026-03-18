# tokidev

Content repository for the tokidev brand. All content is written
in Markdown.

## Structure

```
articles/
  published/    - Published articles
  drafts/       - Work-in-progress articles
  plans/        - Article plans and outlines
  templates/    - Reusable article templates

carousel/
  published/    - Approved and posted carousels
  drafts/       - Work-in-progress carousels
  plans/        - Carousel content plans
  templates/    - Reusable slide structure templates

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

## Content Types

**Articles** — long-form technical blog posts published on the
tokidev blog.

**Carousel** — slide-based content for Instagram and LinkedIn.

**Instagram** — post captions, copy, and hashtag sets for the
@t0k1dev account.

**Video** — scripts for YouTube videos and Reels.

**Brand** — voice guidelines, tone documentation, and brand assets.

## Conventions

- Filenames: kebab-case slugs, no dates or prefixes.
- Every file starts with YAML frontmatter.
- Prose wraps at 80 characters (exceptions: URLs, code blocks,
  frontmatter values).
- See `AGENTS.md` for full conventions, frontmatter schemas, and
  per-type workflows.
