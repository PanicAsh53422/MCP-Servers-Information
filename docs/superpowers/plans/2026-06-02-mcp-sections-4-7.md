# MCP Sections 4–7 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Fill in the four remaining sections of `index.html` (Popular Servers, Use Cases, Getting Started, Code Examples) plus sidebar scroll-spy, all as a self-contained static file.

**Architecture:** Single `index.html` — all CSS added to the existing `<style>` block, all HTML added inside `.content`, all JS added to the existing `<script>` block. Two new CSS components (step guide, tab switcher). No build step, no external libraries beyond the Google Fonts already loaded.

**Tech Stack:** Vanilla HTML, CSS, JavaScript. No frameworks, no bundler.

---

## File Structure

Only one file changes:

- **Modify:** `index.html`
  - `<style>` block (~line 10): add step guide + tab switcher CSS
  - Sidebar stat (~line 319): change `12+` → `7`
  - `.content` div (~line 457): replace section 4 placeholder; add sections 5, 6, 7
  - `<script>` block (~line 467): add tab switcher JS + scroll-spy JS

---

## Task 1: Section 4 — Popular Servers

**Files:**
- Modify: `index.html` (replace the section 4 placeholder, lines ~457–462)

- [ ] **Step 1: Replace the placeholder**

Find and replace this block in `index.html`:

```html
        <!-- remaining sections placeholder -->
        <section class="section" id="popular-servers">
          <div class="section-eyebrow">Section 04</div>
          <div class="section-title">Popular <span>Servers</span></div>
          <div class="section-desc">Coming in the next task.</div>
        </section>
```

Replace with:

```html
        <!-- ── Section 4: Popular Servers ── -->
        <section class="section" id="popular-servers">
          <div class="section-eyebrow">Section 04</div>
          <div class="section-title">Popular <span>Servers</span></div>
          <div class="section-desc">
            The MCP ecosystem has hundreds of servers. These seven are the most useful for classroom work — each installs with a single command and covers a distinct capability.
          </div>
          <div class="cards">
            <div class="card">
              <div class="card-header">
                <div class="card-header-left"><div class="card-num">01</div><div class="card-title">Filesystem — read and write local files</div></div>
                <div class="card-chevron">▶</div>
              </div>
              <div class="card-body">
                The simplest MCP server. Gives Claude access to a directory you specify — it can list, read, write, and search files within it. Great starting point for understanding how MCP tools work.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Have Claude read your notes folder and summarize what you worked on this week.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-filesystem /path/to/folder</code>
              </div>
            </div>
            <div class="card">
              <div class="card-header">
                <div class="card-header-left"><div class="card-num">02</div><div class="card-title">GitHub — repos, PRs, issues, and code</div></div>
                <div class="card-chevron">▶</div>
              </div>
              <div class="card-body">
                Search repositories, read file contents, list and create pull requests, open issues, and post comments — all without leaving Claude. Requires a GitHub personal access token.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Ask Claude to review your latest PR diff and suggest improvements before you merge.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-github</code>
              </div>
            </div>
            <div class="card">
              <div class="card-header">
                <div class="card-header-left"><div class="card-num">03</div><div class="card-title">Postgres — SQL queries against a live database</div></div>
                <div class="card-chevron">▶</div>
              </div>
              <div class="card-body">
                Runs read-only SQL queries against a PostgreSQL database and returns results as plain text. Claude can inspect schema, write and explain queries, and translate business questions into SQL.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Hand Claude an unfamiliar schema and ask it to explain each table in plain English.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-postgres postgresql://localhost/mydb</code>
              </div>
            </div>
            <div class="card">
              <div class="card-header">
                <div class="card-header-left"><div class="card-num">04</div><div class="card-title">Brave Search — real-time web results</div></div>
                <div class="card-chevron">▶</div>
              </div>
              <div class="card-body">
                Queries the Brave Search API for live web results. Claude gets titles, URLs, and descriptions — then synthesizes them into a structured answer. Requires a free Brave API key.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Research a topic and have Claude synthesize results from multiple sources in one response.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-brave-search</code>
              </div>
            </div>
            <div class="card">
              <div class="card-header">
                <div class="card-header-left"><div class="card-num">05</div><div class="card-title">Puppeteer — browser automation and scraping</div></div>
                <div class="card-chevron">▶</div>
              </div>
              <div class="card-body">
                Controls a headless Chrome instance. Claude can navigate pages, click elements, wait for dynamic content, take screenshots, and extract structured data from any website.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Scrape a product page for pricing data, or screenshot a dashboard for a weekly report.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-puppeteer</code>
              </div>
            </div>
            <div class="card">
              <div class="card-header">
                <div class="card-header-left"><div class="card-num">06</div><div class="card-title">Slack — messages, channels, and users</div></div>
                <div class="card-chevron">▶</div>
              </div>
              <div class="card-body">
                Send and read messages, list channels, look up users, and post to Slack workspaces. Requires a Slack bot token and team ID set as environment variables.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Have Claude monitor a support channel and draft responses to common questions.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-slack</code>
              </div>
            </div>
            <div class="card">
              <div class="card-header">
                <div class="card-header-left"><div class="card-num">07</div><div class="card-title">Fetch — retrieve content from any URL</div></div>
                <div class="card-chevron">▶</div>
              </div>
              <div class="card-body">
                Fetches raw HTML or text from any public URL without opening a browser. Useful for pulling in documentation pages, RSS feeds, or plain-text files directly into your conversation.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Point Claude at an API docs page and ask it to write a client based on what it reads.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-fetch</code>
              </div>
            </div>
          </div>
        </section>
```

- [ ] **Step 2: Verify in browser**

Open `index.html` in a browser. Confirm:
- Section 4 shows "Popular Servers" with 7 accordion cards
- Each card expands on click and shows server description + install command
- Only one card open at a time (existing accordion JS handles this)

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: section 4 — popular servers (7 accordion cards)"
```

---

## Task 2: Section 5 — Use Cases

**Files:**
- Modify: `index.html` (add after the closing `</section>` tag of section 4)

- [ ] **Step 1: Add section 5 after section 4**

Immediately after the closing `</section>` of section 4 (and before `</div>` that closes `.content`), insert:

```html
        <hr class="section-divider" />

        <!-- ── Section 5: Use Cases ── -->
        <section class="section" id="use-cases">
          <div class="section-eyebrow">Section 05</div>
          <div class="section-title">Use <span>Cases</span></div>
          <div class="section-desc">
            MCP servers become powerful when combined. These scenarios show how pairing the right servers with Claude solves real problems you'd otherwise solve manually.
          </div>
          <div class="cards">
            <div class="card">
              <div class="card-header">
                <div class="card-header-left"><div class="card-num">01</div><div class="card-title">Research Assistant</div></div>
                <div class="card-chevron">▶</div>
              </div>
              <div class="card-body">
                <strong style="color:var(--text-mid)">Problem:</strong> You need to research a topic across multiple sources and synthesize findings without copy-pasting from tab to tab.<br><br>
                <strong style="color:var(--text-mid)">Servers:</strong> Brave Search + Fetch<br><br>
                Claude searches the web for your query, fetches the full text of the top results, and returns a structured summary with citations — all in one response.
              </div>
            </div>
            <div class="card">
              <div class="card-header">
                <div class="card-header-left"><div class="card-num">02</div><div class="card-title">Code Reviewer</div></div>
                <div class="card-chevron">▶</div>
              </div>
              <div class="card-body">
                <strong style="color:var(--text-mid)">Problem:</strong> You want AI feedback on a pull request without pasting the whole diff into a chat window.<br><br>
                <strong style="color:var(--text-mid)">Server:</strong> GitHub<br><br>
                Claude fetches the PR diff directly from GitHub, reads the changed files, and returns structured review comments organized by file — pointing to specific lines.
              </div>
            </div>
            <div class="card">
              <div class="card-header">
                <div class="card-header-left"><div class="card-num">03</div><div class="card-title">Database Explorer</div></div>
                <div class="card-chevron">▶</div>
              </div>
              <div class="card-body">
                <strong style="color:var(--text-mid)">Problem:</strong> A teammate handed you a Postgres database with no documentation and you need to understand it fast.<br><br>
                <strong style="color:var(--text-mid)">Server:</strong> Postgres<br><br>
                Claude queries the schema, describes each table in plain English, maps out the relationships, and answers questions like "which table tracks user sessions?" without you writing a single query.
              </div>
            </div>
            <div class="card">
              <div class="card-header">
                <div class="card-header-left"><div class="card-num">04</div><div class="card-title">Web Scraper</div></div>
                <div class="card-chevron">▶</div>
              </div>
              <div class="card-body">
                <strong style="color:var(--text-mid)">Problem:</strong> You need structured data from a website that doesn't offer an API.<br><br>
                <strong style="color:var(--text-mid)">Server:</strong> Puppeteer<br><br>
                Claude navigates to the page, waits for dynamic content to load, extracts the data you describe (prices, names, dates), and returns it as a clean table ready to copy into a spreadsheet.
              </div>
            </div>
            <div class="card">
              <div class="card-header">
                <div class="card-header-left"><div class="card-num">05</div><div class="card-title">Team Automator</div></div>
                <div class="card-chevron">▶</div>
              </div>
              <div class="card-body">
                <strong style="color:var(--text-mid)">Problem:</strong> New GitHub issues pile up and it takes time to triage them, assign labels, and notify the team.<br><br>
                <strong style="color:var(--text-mid)">Servers:</strong> GitHub + Slack<br><br>
                Claude reads open issues, categorizes them by type (bug, feature, question), posts a triage summary to a Slack channel, and suggests label assignments — in one prompt.
              </div>
            </div>
          </div>
        </section>
```

- [ ] **Step 2: Verify in browser**

Reload `index.html`. Confirm:
- Section 5 appears below section 4 with a divider between them
- All 5 use case cards expand correctly
- Sidebar still shows all nav items

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: section 5 — use cases (5 scenario cards)"
```

---

## Task 3: Step Guide CSS + Section 6 — Getting Started

**Files:**
- Modify: `index.html` — CSS in `<style>` block; HTML after section 5

- [ ] **Step 1: Add step guide CSS**

Inside the `<style>` block, immediately before the closing `</style>` tag, add:

```css
    /* ── Step Guide ── */
    .steps { display: flex; flex-direction: column; }
    .step {
      display: flex;
      gap: 18px;
      padding: 20px 0;
      border-bottom: 1px solid var(--border);
    }
    .step:last-child { border-bottom: none; }
    .step-num {
      width: 28px; height: 28px;
      border-radius: 50%;
      background: rgba(56,189,248,0.08);
      border: 1px solid rgba(56,189,248,0.2);
      display: flex; align-items: center; justify-content: center;
      font-family: var(--font-code);
      font-size: 11px; font-weight: 700;
      color: var(--accent);
      flex-shrink: 0;
      margin-top: 2px;
    }
    .step-content { flex: 1; }
    .step-title {
      font-size: 13px; font-weight: 600;
      color: var(--text-primary);
      margin-bottom: 6px;
    }
    .step-desc {
      font-size: 12px;
      color: var(--text-muted);
      line-height: 1.75;
    }
    .code-block {
      background: rgba(2,6,15,0.85);
      border: 1px solid var(--border);
      border-radius: 6px;
      padding: 14px 16px;
      margin-top: 10px;
      font-family: var(--font-code);
      font-size: 11px;
      line-height: 1.7;
      color: var(--accent-light);
      white-space: pre;
      overflow-x: auto;
      display: block;
    }
```

- [ ] **Step 2: Add section 6 HTML after section 5**

Immediately after the closing `</section>` of section 5, insert:

```html
        <hr class="section-divider" />

        <!-- ── Section 6: Getting Started ── -->
        <section class="section" id="getting-started">
          <div class="section-eyebrow">Section 06</div>
          <div class="section-title">Getting <span>Started</span></div>
          <div class="section-desc">
            Connect an MCP server to Claude Desktop in five steps. We'll use the Filesystem server as the example — it requires no API key and works immediately.
          </div>
          <div class="steps">
            <div class="step">
              <div class="step-num">1</div>
              <div class="step-content">
                <div class="step-title">Download Claude Desktop</div>
                <div class="step-desc">Install Claude Desktop from anthropic.com. Available for macOS and Windows. This is the host application that manages your MCP connections.</div>
              </div>
            </div>
            <div class="step">
              <div class="step-num">2</div>
              <div class="step-content">
                <div class="step-title">Open the config file</div>
                <div class="step-desc">
                  The config file location depends on your OS. If the file doesn't exist yet, create it.<br><br>
                  <strong style="color:var(--text-mid)">macOS</strong>
                  <code class="code-block">~/Library/Application Support/Claude/claude_desktop_config.json</code>
                  <strong style="color:var(--text-mid)">Windows</strong>
                  <code class="code-block">%APPDATA%\Claude\claude_desktop_config.json</code>
                </div>
              </div>
            </div>
            <div class="step">
              <div class="step-num">3</div>
              <div class="step-content">
                <div class="step-title">Add a server entry</div>
                <div class="step-desc">
                  Paste the following into your config file. Replace <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">/Users/yourname/Documents</code> with an actual folder path on your machine.
                  <code class="code-block">{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/yourname/Documents"
      ]
    }
  }
}</code>
                </div>
              </div>
            </div>
            <div class="step">
              <div class="step-num">4</div>
              <div class="step-content">
                <div class="step-title">Restart Claude Desktop</div>
                <div class="step-desc">Fully quit and relaunch Claude Desktop. The config file is only read on startup — changes won't take effect until you restart.</div>
              </div>
            </div>
            <div class="step">
              <div class="step-num">5</div>
              <div class="step-content">
                <div class="step-title">Verify it's working</div>
                <div class="step-desc">Open a new conversation and type: <em>"List the files in my Documents folder."</em> Claude should call the <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">list_directory</code> tool and return the results. You'll see a tool-use indicator appear in the chat.</div>
              </div>
            </div>
          </div>
        </section>
```

- [ ] **Step 3: Verify in browser**

Reload `index.html`. Confirm:
- Section 6 appears below section 5 with a divider
- Step numbers are circular with accent color
- Code blocks (OS paths, JSON config) are dark with monospace font and horizontal scroll if needed
- Steps are always visible (not collapsible)

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: section 6 — getting started (step guide + config snippet)"
```

---

## Task 4: Tab Switcher CSS + Section 7 — Code Examples

**Files:**
- Modify: `index.html` — CSS in `<style>` block; HTML after section 6

- [ ] **Step 1: Add tab switcher CSS**

Inside the `<style>` block, immediately before `</style>` (after the step guide CSS added in Task 3), add:

```css
    /* ── Tab Switcher ── */
    .tabs {
      display: flex;
      gap: 4px;
      margin-bottom: 16px;
    }
    .tab-btn {
      padding: 6px 18px;
      border-radius: 20px;
      font-size: 11px; font-weight: 600;
      border: 1px solid var(--border);
      background: transparent;
      color: var(--text-muted);
      cursor: pointer;
      font-family: var(--font-code);
      transition: all 0.2s ease;
    }
    .tab-btn:hover { color: var(--accent-light); border-color: rgba(56,189,248,0.2); }
    .tab-btn.active {
      background: rgba(56,189,248,0.1);
      border-color: rgba(56,189,248,0.3);
      color: var(--accent);
    }
    .tab-panel { display: none; }
    .tab-panel.active { display: block; }
    .code-block-label {
      font-size: 9px;
      text-transform: uppercase;
      letter-spacing: 1.5px;
      color: var(--text-dim);
      margin-bottom: 6px;
    }
```

- [ ] **Step 2: Add section 7 HTML after section 6**

Immediately after the closing `</section>` of section 6, insert:

```html
        <hr class="section-divider" />

        <!-- ── Section 7: Code Examples ── -->
        <section class="section" id="code-examples">
          <div class="section-eyebrow">Section 07</div>
          <div class="section-title">Code <span>Examples</span></div>
          <div class="section-desc">
            A minimal MCP server with one tool: <code style="font-family:var(--font-code);color:var(--accent);font-size:12px">search_web(query)</code>. It calls the Brave Search API and returns the top five results. Pick your language — the structure is identical in both.
          </div>
          <div class="tab-group">
            <div class="tabs">
              <button class="tab-btn active" data-lang="python">Python</button>
              <button class="tab-btn" data-lang="typescript">TypeScript</button>
            </div>
            <div class="tab-panel active" data-lang="python">
              <div class="code-block-label">Install: pip install mcp httpx</div>
              <code class="code-block">import httpx
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("brave-search")

BRAVE_API_KEY = "your-api-key-here"

@mcp.tool()
def search_web(query: str) -> str:
    """Search the web using Brave Search and return top 5 results."""
    response = httpx.get(
        "https://api.search.brave.com/res/v1/web/search",
        headers={"X-Subscription-Token": BRAVE_API_KEY},
        params={"q": query, "count": 5},
    )
    results = response.json().get("web", {}).get("results", [])
    return "\n\n".join(
        f"{r['title']}\n{r['url']}\n{r.get('description', '')}"
        for r in results
    )

if __name__ == "__main__":
    mcp.run()</code>
            </div>
            <div class="tab-panel" data-lang="typescript">
              <div class="code-block-label">Install: npm install @modelcontextprotocol/sdk zod</div>
              <code class="code-block">import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";

const BRAVE_API_KEY = "your-api-key-here";

const server = new McpServer({ name: "brave-search", version: "1.0.0" });

server.tool(
  "search_web",
  "Search the web using Brave Search and return top 5 results.",
  { query: z.string().describe("The search query") },
  async ({ query }) => {
    const url = new URL("https://api.search.brave.com/res/v1/web/search");
    url.searchParams.set("q", query);
    url.searchParams.set("count", "5");

    const res = await fetch(url.toString(), {
      headers: { "X-Subscription-Token": BRAVE_API_KEY },
    });
    const data = await res.json();
    const results = data?.web?.results ?? [];

    const text = results
      .map((r) => `${r.title}\n${r.url}\n${r.description ?? ""}`)
      .join("\n\n");

    return { content: [{ type: "text", text }] };
  }
);

const transport = new StdioServerTransport();
await server.connect(transport);</code>
            </div>
          </div>
        </section>
```

- [ ] **Step 3: Verify in browser**

Reload `index.html`. Confirm:
- Section 7 appears with a "Python" and "TypeScript" pill button row
- Python panel shows by default
- Clicking TypeScript switches to the TypeScript panel (tab JS not wired yet — panel will not switch until Task 5; that's expected)

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: section 7 — code examples (tab switcher HTML + CSS)"
```

---

## Task 5: Tab Switcher JS + Scroll-Spy + Sidebar Stat Fix

**Files:**
- Modify: `index.html` — JS in `<script>` block; sidebar stat `<div>`

- [ ] **Step 1: Fix sidebar stat**

Find this line in the sidebar (around line 319):

```html
        <div class="stat-num">12+</div><div class="stat-lbl">Servers</div>
```

Change `12+` to `7`:

```html
        <div class="stat-num">7</div><div class="stat-lbl">Servers</div>
```

- [ ] **Step 2: Add tab switcher and scroll-spy JS**

Inside the `<script>` block, after `initAccordion();`, add:

```javascript
    // Tab switcher
    document.querySelectorAll('.tab-btn').forEach(btn => {
      btn.addEventListener('click', () => {
        const group = btn.closest('.tab-group');
        group.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
        group.querySelectorAll('.tab-panel').forEach(p => p.classList.remove('active'));
        btn.classList.add('active');
        group.querySelector(`.tab-panel[data-lang="${btn.dataset.lang}"]`).classList.add('active');
      });
    });

    // Scroll-spy
    const navItems = document.querySelectorAll('.nav-item[data-section]');
    const spy = new IntersectionObserver(entries => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          navItems.forEach(item => item.classList.remove('active'));
          const hit = document.querySelector(`.nav-item[data-section="${entry.target.id}"]`);
          if (hit) hit.classList.add('active');
        }
      });
    }, { threshold: 0.3 });
    document.querySelectorAll('section[id]').forEach(s => spy.observe(s));
```

- [ ] **Step 3: Verify in browser**

Reload `index.html`. Confirm:
- Sidebar stat reads `7` (not `12+`)
- Clicking "TypeScript" in section 7 switches the code panel; clicking "Python" switches back
- Scrolling down the page updates the active sidebar nav item to match the section in view

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: tab switcher JS, scroll-spy, sidebar stat fix"
```

---

## Self-Review Checklist

| Spec requirement | Task |
|---|---|
| Section 4 — 7 server accordion cards | Task 1 |
| Section 5 — 5 use case accordion cards | Task 2 |
| Section 6 — step guide CSS + 5-step HTML | Task 3 |
| Section 7 — tab switcher CSS + Python + TS panels | Task 4 |
| Tab switcher JS | Task 5 |
| Scroll-spy JS | Task 5 |
| Sidebar stat `12+` → `7` | Task 5 |
| Static file only — no build step, no CDN libraries | All tasks (verified: no new external dependencies added) |
