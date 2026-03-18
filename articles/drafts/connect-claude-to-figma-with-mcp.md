---
title: "Connect Claude to Figma with MCP"
slug: connect-claude-to-figma-with-mcp
date: 2026-03-18
author: t0k1dev
tags: [ai, devtools, figma, mcp, tutorial, claude]
status: draft
description: >
  A practical guide to connecting Claude to Figma using
  figma-console-mcp — two paths, step by step, with
  honest notes on what works and what does not.
---

You type "Extract all the color tokens from this file and
show me the Light and Dark mode values side by side" and
Claude does it. You type "Create a card component with a
header image, title, and action button" and it appears in
your Figma canvas. No copy-pasting, no tab switching, no
export-import cycle.

That is what figma-console-mcp makes possible. It is an
MCP server — a small process that gives Claude a set of
tools it can call to interact with Figma directly. Read
files, extract variables, create frames, manage tokens.
The setup takes about ten minutes.

There are two paths. If you have Node.js installed, use
the NPX path — you get the full toolset, 59+ tools,
including real-time plugin debugging and design creation.
If you want to start from Claude.ai in the browser
without installing anything locally, use Cloud Mode — 43
tools, write access included, no terminal required. Both
paths are covered below. Pick the one that fits.

## What figma-console-mcp is

MCP (Model Context Protocol) is a standard for giving AI
assistants access to external tools. figma-console-mcp is
an open source MCP server — MIT licensed, 1.1k stars on
GitHub — that bridges Claude to Figma's Plugin API and
REST API.

When it is running, Claude gains a set of tools it can
call during a conversation:

- Read design tokens, variables, and their values across
  modes
- Extract component data, styles, and visual specs
- Create frames, shapes, and text directly in a Figma file
- Manage variable collections and modes (add, rename,
  delete)
- Take screenshots of the current canvas for visual context
- Check design-code parity between a component and its
  implementation

Claude decides when to call these tools based on what you
ask. You do not invoke them manually.

## Prerequisites

Before starting, check which path you are taking.

**NPX path:**
- Node.js 18 or later — check with `node --version`
- Figma Desktop (the native app, not the web app)
- A Figma Personal Access Token
- Claude Code CLI or Claude Desktop

**Cloud Mode path:**
- Figma Desktop (the native app, not the web app)
- A Figma Personal Access Token
- A Claude.ai account

Both paths require Figma Desktop. The bridge plugin that
connects Claude to your files only works in the desktop
client, not in the browser.

## Path A: NPX setup

This gives you the full toolset: 59+ tools, real-time
console monitoring, and design creation. Recommended if
you have Node.js available.

### Step 1: Get a Figma Personal Access Token

In Figma, open your account menu in the top-left corner
and go to **Settings**. Under the **Security** section,
find **Personal access tokens** and click **Generate new
token**.

Give it a description — "figma-console-mcp" works — and
copy the token immediately. It starts with `figd_` and
you will not see it again after you close the dialog.

### Step 2: Add the MCP server to your client

**Claude Code:**

```bash
claude mcp add figma-console -s user \
  -e FIGMA_ACCESS_TOKEN=figd_YOUR_TOKEN_HERE \
  -e ENABLE_MCP_APPS=true \
  -- npx -y figma-console-mcp@latest
```

Replace `figd_YOUR_TOKEN_HERE` with the token you just
copied. This command registers the server and stores the
config in `~/.claude.json`.

**Claude Desktop:**

Find the config file for your platform:

| App | macOS | Windows |
|---|---|---|
| Claude Desktop | `~/Library/Application Support/Claude/claude_desktop_config.json` | `%APPDATA%\Claude\claude_desktop_config.json` |
| Cursor | `~/.cursor/mcp.json` | `%USERPROFILE%\.cursor\mcp.json` |
| Windsurf | `~/.codeium/windsurf/mcp_config.json` | `%USERPROFILE%\.codeium\windsurf\mcp_config.json` |

Open the file in any text editor and add the following
inside the `mcpServers` object. Create the file if it
does not exist yet.

```json
{
  "mcpServers": {
    "figma-console": {
      "command": "npx",
      "args": ["-y", "figma-console-mcp@latest"],
      "env": {
        "FIGMA_ACCESS_TOKEN": "figd_YOUR_TOKEN_HERE",
        "ENABLE_MCP_APPS": "true"
      }
    }
  }
}
```

### Step 3: Install the Desktop Bridge plugin

The Desktop Bridge plugin is the piece that runs inside
Figma and relays commands from the MCP server to your
canvas. You install it once and it stays in your
development plugins list.

First, find where NPX installed the package:

```bash
npx figma-console-mcp@latest --print-path
```

This prints an absolute path. Note it.

In Figma Desktop, go to **Plugins → Development → Import
plugin from manifest**. Navigate to the path from above
and select `figma-desktop-bridge/manifest.json`.

Open a Figma file and run the plugin from
**Plugins → Development → Figma Desktop Bridge**. It
connects automatically via WebSocket. You will see a
status indicator in the plugin panel.

### Step 4: Restart your MCP client

Quit and reopen Claude Desktop, Claude Code, or whichever
client you configured. The new server will load on start.

### Step 5: Verify the connection

With the Desktop Bridge plugin running in a Figma file,
ask Claude:

```
Check Figma status
```

You should see a response showing an active WebSocket
connection. Then try the first real test:

```
Create a simple frame with a blue background
```

If a frame appears on your canvas, write access is working.

## Path B: Cloud Mode

No Node.js, no terminal. Works from Claude.ai in the
browser. You get 43 tools including full write access —
design creation, variable management, component
instantiation. What you lose compared to NPX: real-time
console log monitoring and live selection tracking.

### Step 1: Get a Figma Personal Access Token

Same as Path A above. Settings → Security → Generate new
token. Copy it immediately.

### Step 2: Add the connector in Claude.ai

In Claude.ai, open **Settings → Connectors → Add Custom
Connector**.

- **Name:** figma-console
- **URL:** `https://figma-console-mcp.southleft.com/mcp`
- **Authentication:** Bearer token — paste your Figma PAT

Save the connector.

### Step 3: Install the Desktop Bridge plugin

Same plugin as Path A. In Figma Desktop, go to
**Plugins → Development → Import plugin from manifest**
and import from the figma-console-mcp package directory.

If you do not have Node.js to run `--print-path`, clone
the repo and point the import at the manifest file:

```bash
git clone https://github.com/southleft/figma-console-mcp
```

Then import from
`figma-console-mcp/figma-desktop-bridge/manifest.json`.

### Step 4: Pair Claude.ai with the plugin

Open a Figma file and run the Desktop Bridge plugin. In
the plugin panel, toggle **Cloud Mode** on.

In Claude.ai, ask:

```
Connect to my Figma plugin
```

Claude will give you a 6-character pairing code. The code
expires in 5 minutes. Enter it in the plugin panel and
click **Connect**.

Once paired, Claude has write access to the open Figma
file.

### Step 5: Test it

```
Create a card component with a title, description, and
action button
```

A frame should appear on your canvas.

The pairing persists for the session. If you restart Figma
or the plugin, ask Claude for a new pairing code.

## Things you can do now

A few prompts to show the range. These work on both paths
unless noted.

**Read design data:**

```
Get all design variables from [your Figma file URL]
```

```
Extract the color styles from this file
```

```
Get the Button component with a visual reference image
```

**Create designs:**

```
Create a login form with email, password, and a submit
button — 32px padding, 8px border radius
```

```
Design a notification toast with an icon on the left,
title and body text, and a dismiss button
```

```
Build a navigation bar with a logo on the left, centered
menu items, and a user avatar on the right
```

**Manage tokens:**

```
Create a color collection called "Brand Colors" with
Light and Dark modes
```

```
Add a primary color variable — #3B82F6 for Light,
#60A5FA for Dark
```

```
Add a "High Contrast" mode to the existing token
collection
```

**Debug plugins (NPX only):**

```
Watch the console for 30 seconds while I test my plugin
```

```
Show me any errors from the current plugin
```

For the full list of available tools, see the
[TOOLS.md](https://github.com/southleft/figma-console-mcp/blob/main/docs/TOOLS.md)
in the repo.

## When things go wrong

**The plugin does not connect.** You are probably using
Figma in the browser. The Desktop Bridge plugin requires
the native Figma Desktop app. The web app does not support
it.

**"Check Figma status" returns no active connection.** The
plugin needs to be running in Figma Desktop at the same
time as your MCP client. Open a file, run the plugin from
Plugins → Development, then retry.

**Cloud Mode pairing code does not work.** Codes expire
after 5 minutes. Ask Claude for a new one. Make sure the
plugin is in Cloud Mode before entering the code — the
toggle is in the plugin panel.

**"Cannot read design variables" or variables are empty.**
Some variable operations go through the Desktop Bridge
plugin, not the REST API. The plugin needs to be running
in the same file you are targeting. Restart it and try
again.

**Token rejected or auth error.** Check that the token
starts with `figd_` and was not truncated when pasting.
Figma tokens are long — if yours ends abruptly, regenerate
it. For Cloud Mode, make sure you entered it as a Bearer
token, not in a query string or header name field.

**Port conflict on NPX.** If you run two MCP clients at
the same time (say, Claude Desktop and Claude Code), they
will compete for port 9223. The server handles this
automatically — it falls back through ports 9224 to 9232.
If you see a port warning, it is not an error. Both
instances will work.

For anything not covered here, check the
[GitHub issues](https://github.com/southleft/figma-console-mcp/issues).
The repo is actively maintained.

## Wrap-up

You now have Claude connected to Figma. It can read your
design files, extract tokens, create components, and manage
variables — all from a conversation.

One honest note: AI-generated designs are good for
scaffolding and fast iteration. A prompt like "create a
login form" gives you a reasonable starting point, not a
production-ready component. Expect to clean up spacing,
swap in real typography, and adjust things to match your
design system. The tool earns its time back on extraction
and token management, where the output is precise rather
than generative.

A good next thing to try: ask Claude to audit your design
system. "Audit the design system from [file URL]" triggers
the Design System Dashboard — a scored report across
naming, tokens, components, and consistency. It is a
useful way to see what the tool can surface when it has
full read access to a file.
