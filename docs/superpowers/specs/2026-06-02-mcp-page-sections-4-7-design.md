# MCP Servers Page — Sections 4–7 Design

**Date:** 2026-06-02  
**Project:** MCP Servers teaching page (`index.html`)  
**Audience:** Intermediate students at Dixie Tech, proficient in Python and TypeScript  
**Constraint:** Single static `index.html` — no build step, no dev server, no CDN libraries. Pure vanilla JS + inline CSS.

---

## Context

Sections 1–3 (What is MCP, How it Works, Core Capabilities) are complete. The accordion card component, sidebar navigation, and design system are all established. This spec covers sections 4–7 and the sidebar scroll-spy.

---

## Section 4 — Popular Servers

**Pattern:** Existing accordion card layout (no new components)  
**Cards:** 7

| # | Server | Key use |
|---|--------|---------|
| 01 | Filesystem | Read/write local files — simplest possible server |
| 02 | GitHub | Repos, PRs, issues, file contents |
| 03 | Postgres | Run SQL queries against a live database |
| 04 | Brave Search | Web search results via Brave API |
| 05 | Puppeteer | Browser automation, screenshots, scraping |
| 06 | Slack | Send messages, read channels, list users |
| 07 | Fetch | Retrieve raw content from any URL |

Each card body includes: one-sentence description, a classroom use-case sentence, and the install command (`npx` or `pip`).

---

## Section 5 — Use Cases

**Pattern:** Existing accordion card layout  
**Cards:** 5, written as problem → MCP solution scenarios

1. **Research Assistant** — Use Brave Search to pull live results into a summary
2. **Code Reviewer** — Use GitHub server to fetch a PR diff and get AI feedback
3. **Database Explorer** — Use Postgres to query and explain a schema in plain English
4. **Web Scraper** — Use Puppeteer to grab structured data from a live page
5. **Team Automator** — Combine Slack + GitHub to triage issues and post updates

Each card: 2–3 sentences describing the problem, the MCP servers involved, and what the AI does with them.

---

## Section 6 — Getting Started

**Pattern:** New numbered step guide component (not accordion)  
**Perspective:** Using MCP (configuring Claude Desktop), with a brief note on building your own  

### Step guide component design

- Vertical list of numbered steps, each with a title and body
- Steps are always visible (not collapsible) — students follow top to bottom
- Inline `<pre><code>` block for the JSON config snippet, styled with the existing `--font-code` and a dark background
- No interactivity required

### Steps

1. **Download Claude Desktop** — install from anthropic.com (mac/windows)
2. **Open the config file** — path differs by OS; show both (`~/Library/...` on mac, `%APPDATA%\...` on windows)
3. **Add a server entry** — embed the actual `claude_desktop_config.json` JSON example (Filesystem server as the example — simplest)
4. **Restart Claude Desktop** — changes take effect on restart
5. **Verify it's working** — open a new chat, ask Claude to list files in a folder; it should use the filesystem tool

### Config snippet (Filesystem example)

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/Users/yourname/Documents"]
    }
  }
}
```

---

## Section 7 — Code Examples

**Pattern:** New tab switcher component  
**Languages:** Python | TypeScript  
**Example:** A minimal MCP server exposing one tool — `search_web(query: string)` that calls the Brave Search API

### Tab switcher component design

- Two pill-style tab buttons: `Python` and `TypeScript`
- Active tab has accent color + underline; inactive is muted
- Clicking a tab swaps the visible `<pre><code>` block — pure JS classList toggle
- Code blocks: `white-space: pre`, `overflow-x: auto`, dark background, `--font-code`, line-height 1.6
- No syntax highlighting library — code is clear enough at this level without it
- A brief paragraph above the tabs explains what the example does before students read the code

### Python example outline

```python
# Uses: mcp (pip install mcp), httpx
# Defines: search_web(query) tool
# Shows: @mcp.tool() decorator, tool handler, mcp.run()
```

### TypeScript example outline

```typescript
// Uses: @modelcontextprotocol/sdk (npm install)
// Defines: search_web(query) tool
// Shows: server.tool() registration, handler, StdioServerTransport, server.connect()
```

Both examples are self-contained, ~30–40 lines, commented in plain English. No API key is needed to read and understand them (the Brave API call is shown but the key is a placeholder string).

---

## Sidebar Scroll-Spy

The sidebar nav items already have `data-section` attributes matching section IDs. Wire up an `IntersectionObserver` to add/remove the `.active` class as sections scroll into view. Threshold: `0.3` (section is 30% visible). This is pure vanilla JS added to the existing `<script>` block.

---

## What Is Not In Scope

- Any server-side code or build pipeline
- Syntax highlighting library (styled `<pre>` blocks are sufficient)
- Interactive "try it" sandboxes
- Student exercises or quiz elements
- Mobile/responsive layout changes

---

## File Changes

Single file: `index.html`  
- Add sections 4–7 HTML inside `.content`
- Add step guide and tab switcher CSS to `<style>`
- Add tab switcher and scroll-spy JS to `<script>`
- Update sidebar stat: `12+ Servers` → `7` (we cover 7 servers in section 4; the inflated number would confuse students)
