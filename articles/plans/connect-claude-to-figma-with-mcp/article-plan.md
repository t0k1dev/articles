# Plan: Connect Claude to Figma with MCP

## Goal

Write a practical, step-by-step tutorial that shows developers
and designers how to connect Claude to Figma using the
figma-console-mcp server. The reader should be able to follow
along and have a working setup by the end.

## Target reader

Developers or designers who use Claude and Figma regularly
but have never set up an MCP server. They are comfortable
with terminals but not necessarily familiar with the MCP
ecosystem. They want real results, not a conceptual overview.

## Core argument

MCP turns Claude from a chat assistant into a design
collaborator that can read, create, and modify Figma files
directly. The setup is shorter than most readers expect.

## Tone

Tutorial-first. Practical, direct, and confident. No
marketing language about "unlocking the power of AI." Just:
here is what this does, here is how to set it up, here is
what you can do with it.

The voice should feel like a colleague who just set this up
themselves and is walking you through it — not a product
announcement and not a dry documentation page.

Rules for the voice:
- Commands and config go in code blocks, always.
- When something can go wrong, say so plainly and give the
  fix. Do not pretend the happy path always works.
- Concrete before conceptual. Show the setup first, explain
  the architecture after — or not at all if it does not
  help the reader.
- Avoid: "seamlessly", "powerful", "cutting-edge",
  "game-changer", "revolutionize", "unlock".
- No filler transitions. "Now" and "next" are enough.

---

## Reference material

- GitHub repo: https://github.com/southleft/figma-console-mcp
- YouTube video (Spanish): https://www.youtube.com/watch?v=gZIVqksWSV4
  Title: "Diseña con Claude en Figma (sin terminal) IA y MCP
  para diseño UX/UI" — covers Cloud Mode without Node.js

---

## Setup methods to cover

The repo offers four connection methods. The article should
focus on two:

1. **NPX (recommended)** — full 59+ tools, requires Node.js,
   works with Claude Code and Claude Desktop.
2. **Cloud Mode** — 43 tools, no Node.js required, works
   with Claude.ai in the browser. Lower barrier to entry.
   This is what the YouTube video covers.

Remote SSE (read-only, 22 tools) and Local Git (contributor
setup) are out of scope. Mention they exist but do not walk
through them.

---

## Sections

### 1. Introduction (no heading)

- Open with what this actually does: Claude can open a Figma
  file, read its components and variables, create frames, and
  modify designs — directly from a conversation.
- One concrete example before any setup instructions. Either
  a prompt and its result or a before/after. Something that
  makes the reader want to keep reading.
- State the two paths upfront: with Node.js (NPX, full
  access) and without (Cloud Mode, browser-based). Let the
  reader pick their path.
- Keep it short. Two to three paragraphs maximum.

### 2. What figma-console-mcp is

- Explain MCP in one sentence — do not over-explain the
  protocol. The reader needs just enough to understand what
  is happening.
- What the server actually does: bridges Claude to Figma's
  Plugin API and REST API. Gives Claude tools it can call:
  read variables, create frames, take screenshots, etc.
- List the key capabilities briefly:
  - Read design tokens and variables
  - Extract component data and styles
  - Create and modify frames, shapes, and text
  - Manage variable collections and modes
  - Take screenshots for visual context
- Do not list all 59 tools. Pick the five or six most useful
  for the typical use case.
- Mention it is open source (MIT), actively maintained,
  1.1k stars on GitHub.

### 3. Prerequisites

What the reader needs before starting:

**For NPX path:**
- Node.js 18+ (link to nodejs.org)
- Figma Desktop (not the web app — the desktop client)
- A Figma Personal Access Token
- Claude Code CLI or Claude Desktop

**For Cloud Mode path:**
- Figma Desktop
- A Figma Personal Access Token
- Claude.ai account (free tier works for testing)

Include the one-liner to check Node.js version:
`node --version`

### 4. Path A: NPX setup (recommended for full access)

Walk through the NPX setup step by step. Match the structure
of the official README but with more explanation at each step.

**Step 1: Get a Figma Personal Access Token**
- Where to find it in Figma (Settings → Security)
- What permissions it needs
- It starts with `figd_` — save it, you will not see it again

**Step 2: Add the MCP server to your client**

For Claude Code:
```bash
claude mcp add figma-console -s user \
  -e FIGMA_ACCESS_TOKEN=figd_YOUR_TOKEN_HERE \
  -e ENABLE_MCP_APPS=true \
  -- npx -y figma-console-mcp@latest
```

For Claude Desktop, show the JSON config block with the
path to the config file on macOS and Windows.

**Step 3: Install the Desktop Bridge plugin**

- Open Figma Desktop
- Plugins → Development → Import plugin from manifest
- Get the manifest path: `npx figma-console-mcp@latest --print-path`
- Select `figma-desktop-bridge/manifest.json`
- Run the plugin — it auto-connects

**Step 4: Restart your MCP client**

**Step 5: Verify the connection**

Test prompt: "Check Figma status"
Expected: connection status with active WebSocket transport.

Then the first real test: "Create a simple frame with a blue
background." If this works, write access is confirmed.

### 5. Path B: Cloud Mode (no terminal required)

For readers who want to start without installing anything
locally. The YouTube video covers this path.

**Step 1: Get a Figma Personal Access Token** (same as above)

**Step 2: Add the MCP connector in Claude.ai**
- Settings → Connectors → Add Custom Connector
- URL: `https://figma-console-mcp.southleft.com/mcp`
- Auth: Figma PAT as Bearer token

**Step 3: Install the Desktop Bridge plugin in Figma Desktop**
(same plugin as NPX path — it supports both modes)

**Step 4: Pair Claude.ai with the plugin**
- Open the Desktop Bridge plugin in Figma
- Ask Claude: "Connect to my Figma plugin"
- Claude gives a 6-character pairing code (expires 5 min)
- In the plugin: toggle "Cloud Mode" → enter the code →
  click Connect

**Step 5: Test it**
Prompt: "Create a card component with a title and description"

Note the limitations of Cloud Mode vs. NPX:
- No real-time monitoring (console logs, selection tracking)
- 43 tools instead of 59+
- Variables work on all plans (uses Plugin API, not
  Enterprise REST API)

### 6. Things you can do now

A small set of concrete prompts to show the range of
what is possible. Not a full reference — just enough to
spark ideas.

**Read design data:**
```
Get all design variables from [your Figma file URL]
Extract the color styles from this file
Get the Button component with a visual reference image
```

**Create designs:**
```
Create a login form with email, password, and a submit button
Design a notification toast with an icon and dismiss button
Build a navigation bar with logo, menu items, and a user avatar
```

**Manage tokens:**
```
Create a color collection called "Brand Colors" with Light
and Dark modes
Add a primary color variable — #3B82F6 for Light, #60A5FA
for Dark
Add a "High Contrast" mode to the existing collection
```

**Debug plugins:**
(NPX/Local Git only)
```
Watch the console for 30 seconds while I test my plugin
Show me any console errors from the current plugin
```

Keep this section brief. The goal is to show range, not
to be exhaustive.

### 7. When things go wrong

Cover the common failure points clearly. No softening.

- **Plugin does not connect:** Make sure you are using
  Figma Desktop, not the web app. The web app does not
  support the bridge plugin.
- **Pairing code expired:** Codes expire after 5 minutes.
  Ask Claude for a new one.
- **"Cannot read design variables":** Some variable
  operations require the Desktop Bridge plugin to be
  running in the same file you are targeting.
- **NPX method, port conflict:** If two MCP clients are
  running simultaneously, the server auto-falls back to
  ports 9224–9232. This is normal.
- **Token rejected:** Check that the token starts with
  `figd_` and was not accidentally truncated when pasting.
- Where to go for help: GitHub issues at the repo URL.

### 8. Wrap-up

- Short. Two to three paragraphs.
- Recap what the reader now has: Claude connected to Figma,
  able to read and write designs through conversation.
- Honest note on where this is still rough: AI-generated
  designs often need manual cleanup. The tool is best for
  scaffolding, extraction, and iteration — not for
  pixel-perfect final output from a single prompt.
- Close with something concrete: the next prompt to try, or
  the next capability worth exploring (design-code parity
  checking, design system auditing via the Dashboard MCP app).

---

## What to avoid

- Do not explain the MCP protocol in depth. One sentence
  is enough. The reader wants to use it, not understand the
  spec.
- Do not list all 59 tools. Link to the docs for that.
- Do not compare to other Figma AI tools unless it adds
  clarity — this article is about setup and use, not
  market positioning.
- Do not use the YouTube video as a primary source for
  steps — verify all steps against the GitHub README and
  test them. The video covers Cloud Mode and may be outdated.
- No screenshots of the Figma UI in the article text —
  the plugin UI may change. Use text descriptions instead.

---

## Narrative arc

Introduction → "here is what this actually does"
Section 2 → quick context on the tool
Section 3 → what you need before you start
Section 4 → NPX path, step by step
Section 5 → Cloud Mode path, step by step
Section 6 → what to do with it
Section 7 → when it breaks
Section 8 → short close

The arc should feel like: here is the payoff → here is
what it is → here is how to get it → here is what to do
next → here is what to do when it goes wrong.

---

## Writing checklist

- [ ] Frontmatter complete (title, slug, date, author, tags,
      status, description)
- [ ] Introduction has a concrete example before any setup
- [ ] Both paths (NPX and Cloud Mode) are clearly delineated
- [ ] Every terminal command is in a code block
- [ ] Every JSON config block is complete and copy-pasteable
- [ ] Figma token instructions are accurate (check against
      current Figma UI)
- [ ] Desktop Bridge plugin setup is tested and correct
- [ ] Cloud Mode pairing flow is tested and correct
- [ ] Limitations of each path are stated plainly
- [ ] Troubleshooting section covers the most common failures
- [ ] No passive voice for important instructions
- [ ] None of: "seamlessly", "powerful", "game-changer",
      "unlock", "cutting-edge"
- [ ] No AI disclaimers or marketing language
- [ ] Lines wrapped at 80 characters
- [ ] Headings in sentence case
- [ ] Links to the GitHub repo and relevant docs included
