# MCP Guide Site — Content Depth Revision

**Date:** 2026-06-03
**Status:** Approved

## Goal

Revise the content depth across all 7 sections of `index.html` to support a 15-minute class presentation with the site as a visual aid. After the presentation, the site is handed to students as a reference. Content should be scannable during presenting but detailed enough to learn from independently.

## Audience

Students 6 months into a software development course. Fluent in TypeScript and Python. Already building AI-powered software. Need practical guidance on setting up and using MCP servers — not a conceptual intro to programming or APIs.

## Two Changes Applied Everywhere

### 1. Expanded card bodies
Every accordion card body grows from 1–2 sentences to 3–5. Extra sentences connect the concept to what students are already doing — building software with AI — so content feels immediately applicable, not academic.

### 2. Key takeaway block (new styled element)
A small callout block at the bottom of every card body. One plain-English sentence capturing the single most important idea from that card.

**Styling:** left accent border (`--accent` teal), slightly tinted background (`rgba(94, 234, 212, 0.06)`), `Key takeaway:` label in accent color, takeaway text in `--text-mid`. Fits existing dark aesthetic. CSS class: `.card-takeaway`.

Key takeaway blocks are NOT added to step items in Section 6 — steps use a different layout. Section 6 gets one section-level takeaway at the very end instead.

## Section-by-Section Changes

### Section 1 — What is MCP
**Section description:** stays tight — "USB-C for AI" analogy is kept.

**Cards:**
- **Why does MCP exist?** — Expand to explain the "before MCP" problem in terms students recognise: every AI integration was a custom build. Takeaway: write the server once, any compatible client can use it.
- **How is it different from REST?** — Expand to explain the key difference: with REST you decide what to call; with MCP the model decides based on context. Takeaway: MCP is designed for AI autonomy, not human-driven calls.
- **Who created it and who supports it?** — Expand adoption: major editors (VS Code, Cursor, Zed) and AI tools already support it. Takeaway: MCP is young but moving fast — worth learning now.

### Section 2 — How it Works
**Section description:** stays as-is.

**Cards:**
- **The three layers** — Expand to clarify the developer's role: you build the Server; the Host and Client are handled by the AI app the user already has. Takeaway: as a developer, your job is the server — the rest is handled for you.
- **Transport types: stdio vs HTTP+SSE** — Simplify jargon. Rewrite for clarity: stdio = runs locally as a child process, zero config; HTTP+SSE = runs remotely, needed for shared or deployed servers. Takeaway: start with stdio — it works immediately with no networking setup.
- **The request flow** — Keep the numbered list, add a brief sentence before and after explaining that the model never calls your server directly. Takeaway: your server only needs to return the right result — routing and injection are handled by the client.

### Section 3 — Core Capabilities
**Section description:** stays as-is.

**Cards:**
- **Tools** — Expand with a concrete example of a tool definition in plain terms. Clarify that Tools are 90% of what developers build. Takeaway: tools are the main primitive — if you're building an MCP server, you're probably building tools.
- **Resources** — Rewrite with a clearer example relevant to students (e.g. exposing a config file or database row as a readable URI). Takeaway: resources give the model read access to data without it needing to call a function.
- **Prompts** — Expand to clarify when prompts are useful vs. overkill. Takeaway: prompts are the least common primitive — get comfortable with tools first.

### Section 4 — Popular Servers
**Section description:** stays as-is ("hundreds of servers / these seven...").

**Cards (all 7):** Expand the description paragraph (what it actually does, what it's good at). Classroom use examples stay. Takeaway per card: one-liner on when you'd reach for this server in a real project.

Notable specific updates:
- **GitHub** — note that it needs a personal access token — add text like "Requires a GitHub personal access token — see the API Keys step in Getting Started." No inline link needed.
- **Brave Search** — note it needs a free API key with same pattern.
- **Slack** — note it needs a bot token and team ID with same pattern.

### Section 5 — Use Cases
**Section description:** stays as-is.

**Cards (all 5):** Outcome paragraphs expanded to be more concrete and exciting — what does Claude actually return, what does it look like. Takeaway per card: names the reusable pattern (e.g. "The pattern: search + fetch + synthesize").

### Section 6 — Getting Started
**Section description:** stays as-is.

**Steps:** Descriptions expanded slightly for steps that are currently thin (Step 4: Restart, Step 5: Verify). Step 5 gets a note on what the tool-use indicator looks like in the Claude Desktop UI.

**New step — API Keys (inserted between Step 3 and current Step 4; current steps 4 and 5 become 5 and 6):**
- What an API key is: a secret credential that identifies your app to an external service.
- How to add one to the MCP config: use the `env` field in the server entry (not hardcoded in `args`).
- Config example showing `env` with a placeholder key.
- Where to get keys for the most common servers: GitHub (Personal Access Token settings), Brave (free tier at brave.com/search/api), Slack (bot token from api.slack.com/apps).
- Warning: never commit API keys to git — add the config file to `.gitignore` or use a secrets manager.

**Section-level takeaway** (after all steps, not inside a card): "Once you've done this once, adding new servers is just one config block and a restart."

### Section 7 — Code Examples
**Section description:** Expand to explain the three structural pieces of any MCP server — instantiate a server, register tools with a description and schema, connect a transport. This gives students a mental model before they read the code.

**Takeaway** (after the tab switcher, below the code): "Every MCP server follows this same three-part structure — swap in different tool logic and it works for any use case."

## New CSS

Add `.card-takeaway` class:

```css
.card-takeaway {
  margin-top: 18px;
  padding: 10px 14px;
  border-left: 2px solid var(--accent);
  border-radius: 0 4px 4px 0;
  background: rgba(94, 234, 212, 0.06);
  color: var(--text-mid);
  font-size: 14.5px;
  line-height: 1.7;
}

.card-takeaway strong {
  color: var(--accent);
  font-weight: 650;
}
```

And a `.section-takeaway` class for use in Sections 6 and 7:

```css
.section-takeaway {
  margin-top: 28px;
  padding: 14px 18px;
  border-left: 2px solid var(--accent-2);
  border-radius: 0 4px 4px 0;
  background: rgba(96, 165, 250, 0.06);
  color: var(--text-mid);
  font-size: 15px;
  line-height: 1.7;
}

.section-takeaway strong {
  color: var(--accent-2);
  font-weight: 650;
}
```

## Out of Scope

- No structural changes to sections or navigation
- No new sections (API key coverage added as a step inside Section 6)
- No changes to the sidebar stats
- No changes to JavaScript behaviour
- No changes to the code examples themselves (only the surrounding description)
