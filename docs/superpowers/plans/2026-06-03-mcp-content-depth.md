# MCP Guide Site — Content Depth Revision Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Expand all accordion card bodies with richer content, add a styled "Key takeaway" block to every card, insert a new API Keys step in Section 6, and add section-level takeaways to Sections 6 and 7.

**Architecture:** Pure HTML/CSS edits to a single file (`index.html`). No JavaScript changes. CSS classes added in Task 1 unlock styling for all subsequent tasks. Each later task is a self-contained find/replace in one section.

**Tech Stack:** HTML, CSS — no build step. Edit `index.html` directly. Verify each task by opening the file in a browser.

---

## Files

- Modify: `index.html` (all changes)

---

## Task 1: Add CSS Classes

**Files:**
- Modify: `index.html` — add two new CSS rule blocks inside the `<style>` tag

- [ ] **Step 1: Add `.card-takeaway` and `.section-takeaway` CSS**

Find this block near the end of the CSS (around line 950, after the inline-code rule):

```css
    .card-body code:not(.code-block),
    .step-desc code:not(.code-block),
    .section-desc code:not(.code-block) {
      border-color: rgba(94, 234, 212, 0.22);
      background: rgba(94, 234, 212, 0.08);
      color: var(--accent) !important;
    }
```

Add these two rule blocks immediately after it (before the first `@media` query):

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

- [ ] **Step 2: Verify in browser**

Open `index.html` in a browser. Temporarily add `<div class="card-takeaway"><strong>Key takeaway:</strong> Test</div>` inside any card body, expand that card, and confirm: teal left border, faint tinted background, "Key takeaway:" in teal, rest in muted text. Remove the temp element when done.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add card-takeaway and section-takeaway CSS classes"
```

---

## Task 2: Section 1 — What is MCP (3 cards)

**Files:**
- Modify: `index.html` — card bodies inside `#what-is-mcp`

- [ ] **Step 1: Replace card 01 body (Why does MCP exist?)**

Find:
```html
              <div class="card-body">Before MCP, every AI tool integration was a custom one-off build. MCP standardizes how models discover and invoke tools — so a server written once works with any MCP-compatible client: Claude Desktop, Cursor, VS Code, Continue, and more.</div>
```

Replace with:
```html
              <div class="card-body">Before MCP, every AI tool integration was a custom one-off build. If you wanted Claude to read your files, you built a custom integration. If you wanted it to query your database, you built another — a completely separate one that shared nothing with the first. MCP standardizes how models discover and invoke tools, so a server written once works with any MCP-compatible client: Claude Desktop, Cursor, VS Code, Continue, Zed, and more. If you're already building software with AI, MCP is the layer that lets your AI actually take actions in your environment — not just talk about them.<div class="card-takeaway"><strong>Key takeaway:</strong> Write the server once; every MCP-compatible AI client can use it without any changes.</div></div>
```

- [ ] **Step 2: Replace card 02 body (How is it different from REST?)**

Find:
```html
              <div class="card-body">REST APIs are designed for app-to-app communication with a known schema. MCP is designed for AI-to-tool communication — it includes a built-in discovery step where the model queries what tools are available at runtime, then decides which to call based on the user's request.</div>
```

Replace with:
```html
              <div class="card-body">REST APIs are designed for app-to-app communication with a known schema — your code decides which endpoint to call and when. MCP is designed for AI-to-tool communication: it includes a built-in discovery step where the model queries what tools are available at runtime, then decides which to call based on the user's request. You don't write the logic that triggers the tool — the model does, based on what the user asked. Your server doesn't need to know anything about the conversation or the model; it just receives a validated tool call and returns a result. Think of REST as a menu where the developer orders; MCP is a menu where the AI orders.<div class="card-takeaway"><strong>Key takeaway:</strong> With REST, your code decides what to call. With MCP, the model decides — based on the user's intent, not a hardcoded condition.</div></div>
```

- [ ] **Step 3: Replace card 03 body (Who created it?)**

Find:
```html
              <div class="card-body">Created by Anthropic and open-sourced in late 2024. Now supported across Claude Desktop, Cursor, VS Code Copilot Chat, Continue, Zed, and a growing list of AI clients. The spec and SDKs are maintained at <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">github.com/modelcontextprotocol</code>.</div>
```

Replace with:
```html
              <div class="card-body">Created by Anthropic and open-sourced in late 2024. In under a year it became the de facto standard for AI tool integrations — adopted by Cursor, VS Code Copilot Chat, Continue, Zed, and dozens of other clients. Official SDKs exist for Python and TypeScript, both actively maintained and well-documented. The spec is open and versioned at <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">github.com/modelcontextprotocol</code> — you can read exactly how it works, not just how to use it. Because the editors you already use support it, any MCP server you build today works across your whole development environment without additional setup.<div class="card-takeaway"><strong>Key takeaway:</strong> MCP is young but already the standard — the Python and TypeScript SDKs are all you need to get building.</div></div>
```

- [ ] **Step 4: Verify in browser**

Open `index.html`, scroll to Section 1, expand each of the three cards. Confirm: expanded text is readable, takeaway block appears at the bottom of each card with teal left border and "Key takeaway:" label.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: expand Section 1 cards and add key takeaways"
```

---

## Task 3: Section 2 — How it Works (3 cards)

**Files:**
- Modify: `index.html` — card bodies inside `#how-it-works`

- [ ] **Step 1: Replace card 01 body (The three layers)**

Find:
```html
              <div class="card-body">
                <strong style="color:var(--text-mid)">Host</strong> — the application the user runs (e.g. Claude Desktop, Cursor). It manages the user session and holds one or more clients.<br><br>
                <strong style="color:var(--text-mid)">Client</strong> — lives inside the host; maintains a 1:1 connection with one MCP server. Forwards tool calls from the model to the server.<br><br>
                <strong style="color:var(--text-mid)">Server</strong> — a separate process (or remote service) that exposes tools, resources, and prompts. You build this.
              </div>
```

Replace with:
```html
              <div class="card-body">
                <strong style="color:var(--text-mid)">Host</strong> — the application the user runs (e.g. Claude Desktop, Cursor). It manages the user session, renders the conversation UI, and holds one or more clients. You don't build this — you use it.<br><br>
                <strong style="color:var(--text-mid)">Client</strong> — lives inside the host; maintains a 1:1 connection with one MCP server. Forwards tool calls from the model to the server and routes results back. Also handled for you by the host app.<br><br>
                <strong style="color:var(--text-mid)">Server</strong> — a separate process (or remote service) that exposes tools, resources, and prompts. This is what you build. A server can be as small as 30 lines of Python or TypeScript, and it runs independently of whatever AI client the user has.
                <div class="card-takeaway"><strong>Key takeaway:</strong> As a developer, your job is the server — the host and client are handled by whatever AI app the user is already running.</div>
              </div>
```

- [ ] **Step 2: Replace card 02 body (Transport types)**

Find:
```html
              <div class="card-body">
                <strong style="color:var(--text-mid)">stdio</strong> — the host spawns your server as a child process and communicates over stdin/stdout. Used for local servers. Simple, no networking required.<br><br>
                <strong style="color:var(--text-mid)">HTTP + SSE</strong> — server runs as an HTTP service; client connects via Server-Sent Events for streaming. Used for remote or shared servers.
              </div>
```

Replace with:
```html
              <div class="card-body">
                <strong style="color:var(--text-mid)">stdio</strong> — the host launches your server as a child process and communicates through stdin/stdout. Used for local servers running on the same machine. Zero networking setup: no ports, no auth, no firewall rules. This is what you'll use for development and most practical work.<br><br>
                <strong style="color:var(--text-mid)">HTTP + SSE</strong> — your server runs as a regular HTTP service; the client connects via Server-Sent Events for streaming responses. Use this when the server needs to run remotely (a deployed API, a shared team tool) or when multiple users need access to the same server simultaneously.
                <div class="card-takeaway"><strong>Key takeaway:</strong> Start with stdio — it's local, needs no config, and works immediately. Switch to HTTP+SSE only when you need remote or shared access.</div>
              </div>
```

- [ ] **Step 3: Replace card 03 body (The request flow)**

Find:
```html
              <div class="card-body">
                1. User sends a message to the AI.<br>
                2. Model decides a tool call is needed and emits a tool-call request.<br>
                3. Client routes the request to the appropriate MCP server.<br>
                4. Server executes the tool and returns a result.<br>
                5. Result is injected back into the model's context.<br>
                6. Model generates a final response using the result.
              </div>
```

Replace with:
```html
              <div class="card-body">
                When a user sends a message, here's what happens under the hood:<br><br>
                1. User sends a message to the AI.<br>
                2. Model reads the full list of available tools from all connected servers.<br>
                3. Model decides a tool call is needed and emits a structured tool-call request.<br>
                4. Client routes the request to the correct MCP server.<br>
                5. Server executes the tool and returns a result.<br>
                6. Result is injected back into the model's context as a new message.<br>
                7. Model generates a final response using the result.<br><br>
                Your server only ever handles step 5 — it receives a validated call and returns a result. The model, host, and client handle everything else.
                <div class="card-takeaway"><strong>Key takeaway:</strong> Your server handles one step — receive a tool call, return a result. Everything else is managed by the client and host.</div>
              </div>
```

- [ ] **Step 4: Verify in browser**

Open `index.html`, scroll to Section 2, expand each card. Confirm the numbered list in card 03 still renders correctly with line breaks, and all three takeaway blocks appear.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: expand Section 2 cards and add key takeaways"
```

---

## Task 4: Section 3 — Core Capabilities (3 cards)

**Files:**
- Modify: `index.html` — card bodies inside `#core-capabilities`

- [ ] **Step 1: Replace card 01 body (Tools)**

Find:
```html
              <div class="card-body">A tool is a named function with a description and a JSON Schema defining its inputs. The model reads the description to decide when to use it, then passes validated arguments. Tools can do anything: query a database, call an API, run a shell command, write a file.</div>
```

Replace with:
```html
              <div class="card-body">A tool is a named function with a description and a JSON Schema defining its inputs. The model reads the description to decide when to use it, then passes validated arguments — it never guesses at input shapes. Tools can do anything: query a database, call an external API, run a shell command, write a file. In Python, defining a tool is as simple as decorating a function with <code>@mcp.tool()</code> — the SDK infers the schema from your type hints automatically. About 90% of MCP server development is writing tools; resources and prompts are optional extras you can add later.<div class="card-takeaway"><strong>Key takeaway:</strong> Tools are the main primitive — if you're building an MCP server, you're almost certainly building tools.</div></div>
```

- [ ] **Step 2: Replace card 02 body (Resources)**

Find:
```html
              <div class="card-body">Resources are identified by a URI (e.g. <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">file:///path/to/doc.txt</code>) and return text or binary content. Unlike tools, resources are passive — the model or host fetches them, rather than invoking them with arguments. Useful for exposing files, database rows, or documentation.</div>
```

Replace with:
```html
              <div class="card-body">Resources are identified by a URI (e.g. <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">file:///path/to/doc.txt</code> or <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">db://users/42</code>) and return text or binary content. Unlike tools, resources are passive — the model or host fetches them rather than invoking them with arguments. Think of them as read-only data sources: a config file, a database row, a documentation page, a code snippet. Exposing something as a resource makes sense when the data has a stable identifier and doesn't need parameters beyond that ID. In practice, tools are far more commonly used because they're more flexible — but resources are a clean fit when your content maps naturally to a URI.<div class="card-takeaway"><strong>Key takeaway:</strong> Resources give the model read access to named data — use them when your content has a stable URI and doesn't need arguments beyond an ID.</div></div>
```

- [ ] **Step 3: Replace card 03 body (Prompts)**

Find:
```html
              <div class="card-body">A server can expose named prompt templates with dynamic arguments. The host can surface these as slash-commands or quick actions in the UI. Less commonly used than tools, but useful for standardizing repeated workflows — e.g. a "summarize PR" prompt that always fetches the latest diff first.</div>
```

Replace with:
```html
              <div class="card-body">A server can expose named prompt templates with dynamic arguments — like a function that returns a pre-filled conversation starter. The host can surface these as slash-commands or quick actions in the UI (Claude Desktop shows them in the <code>/</code> menu). Prompts make sense when you have a repeated workflow that always starts the same way: "review this PR", "explain this error log", "summarize this week's issues". They're the least commonly used of the three primitives — most servers never define any. Get comfortable building tools first; come back to prompts when you have a clear repeated workflow to standardize.<div class="card-takeaway"><strong>Key takeaway:</strong> Prompts are optional and uncommon — master tools first, then revisit these when you have a concrete repeated workflow.</div></div>
```

- [ ] **Step 4: Verify in browser**

Open `index.html`, scroll to Section 3, expand all three cards. Confirm the inline `<code>` elements in the Tools card render with the teal accent style, and all takeaways appear.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: expand Section 3 cards and add key takeaways"
```

---

## Task 5: Section 4 — Popular Servers (7 cards)

**Files:**
- Modify: `index.html` — card bodies inside `#popular-servers`

- [ ] **Step 1: Replace card 01 body (Filesystem)**

Find:
```html
                The simplest MCP server. Gives Claude access to a directory you specify — it can list, read, write, and search files within it. Great starting point for understanding how MCP tools work.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Have Claude read your notes folder and summarize what you worked on this week.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-filesystem /path/to/folder</code>
```

Replace with:
```html
                The simplest MCP server and the best one to start with. You give it a directory path in the config and Claude gets read/write access to every file inside it — list directories, read file contents, write new files, search by name or content. It's sandboxed to the folder you specify, so you control exactly what Claude can touch. Because it uses only stdio and requires no API key, it's the easiest way to confirm your MCP setup is working before moving on to more complex servers.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Have Claude read your notes folder and summarize what you worked on this week.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-filesystem /path/to/folder</code>
                <div class="card-takeaway"><strong>Key takeaway:</strong> Use this first when learning MCP — no API key needed, and getting it working proves your entire setup is correct.</div>
```

- [ ] **Step 2: Replace card 02 body (GitHub)**

Find:
```html
                Search repositories, read file contents, list and create pull requests, open issues, and post comments — all without leaving Claude. Requires a GitHub personal access token.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Ask Claude to review your latest PR diff and suggest improvements before you merge.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-github</code>
```

Replace with:
```html
                Search repositories, read file contents, list and create pull requests, open issues, post comments, and browse commit history — all from inside Claude. Because it talks directly to the GitHub API, it works on any repo you have access to, public or private. Requires a GitHub personal access token with <code>repo</code> scope — see the API Keys step in Getting Started for how to add it to your config. Once connected, you can ask Claude to review a PR, explain a commit, or draft an issue description without switching apps.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Ask Claude to review your latest PR diff and suggest improvements before you merge.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-github</code>
                <div class="card-takeaway"><strong>Key takeaway:</strong> If you only set up one server beyond Filesystem, make it GitHub — it fits directly into the workflow you already have.</div>
```

- [ ] **Step 3: Replace card 03 body (Postgres)**

Find:
```html
                Runs read-only SQL queries against a PostgreSQL database and returns results as plain text. Claude can inspect schema, write and explain queries, and translate business questions into SQL.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Hand Claude an unfamiliar schema and ask it to explain each table in plain English.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-postgres postgresql://localhost/mydb</code>
```

Replace with:
```html
                Runs read-only SQL queries against a PostgreSQL database and returns results as plain text. Claude can inspect your schema, write queries from plain-English descriptions, explain what existing queries do, and answer questions about your data without you writing a line of SQL. The connection string in your config acts as the credentials — Claude only gets the permissions that connection has, so a read-only user keeps things safe. Useful for learning a new codebase's data model fast, or exploring a database you didn't build.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Hand Claude an unfamiliar schema and ask it to explain each table in plain English.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-postgres postgresql://localhost/mydb</code>
                <div class="card-takeaway"><strong>Key takeaway:</strong> Point Claude at a database and ask questions in plain English — no SQL experience required.</div>
```

- [ ] **Step 4: Replace card 04 body (Brave Search)**

Find:
```html
                Queries the Brave Search API for live web results. Claude gets titles, URLs, and descriptions — then synthesizes them into a structured answer. Requires a free Brave API key.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Research a topic and have Claude synthesize results from multiple sources in one response.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-brave-search</code>
```

Replace with:
```html
                Queries the Brave Search API for live, real-time web results — bypassing Claude's training cutoff. Claude gets page titles, URLs, and descriptions, then synthesizes them into a structured answer. Requires a free Brave Search API key (see the API Keys step in Getting Started). Combine it with the Fetch server for a full research workflow: search returns the URLs, Fetch retrieves the full page text, Claude synthesizes everything into one response.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Research a topic and have Claude synthesize results from multiple sources in one response.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-brave-search</code>
                <div class="card-takeaway"><strong>Key takeaway:</strong> Use this when you need Claude to work with information that's newer than its training data.</div>
```

- [ ] **Step 5: Replace card 05 body (Puppeteer)**

Find:
```html
                Controls a headless Chrome instance. Claude can navigate pages, click elements, wait for dynamic content, take screenshots, and extract structured data from any website.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Scrape a product page for pricing data, or screenshot a dashboard for a weekly report.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-puppeteer</code>
```

Replace with:
```html
                Controls a full headless Chrome browser, not just a plain HTTP fetcher. Claude can navigate to URLs, click buttons, fill forms, wait for JavaScript to finish rendering, take screenshots, and extract structured data from dynamic pages that a simple fetch can't handle. Because it runs real Chrome, it works on React apps, infinite-scroll feeds, and any site that relies heavily on JavaScript to render its content. No API key required.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Scrape a product page for pricing data, or screenshot a dashboard for a weekly report.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-puppeteer</code>
                <div class="card-takeaway"><strong>Key takeaway:</strong> Reach for Puppeteer when a page uses JavaScript to render its content — a plain Fetch won't work on those.</div>
```

- [ ] **Step 6: Replace card 06 body (Slack)**

Find:
```html
                Send and read messages, list channels, look up users, and post to Slack workspaces. Requires a Slack bot token and team ID set as environment variables.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Have Claude monitor a support channel and draft responses to common questions.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-slack</code>
```

Replace with:
```html
                Send and read messages, list channels, look up users, react to messages, and post formatted content to Slack workspaces — all from Claude. Requires a Slack bot token and your team ID set as environment variables (see the API Keys step in Getting Started). You'll need to create a Slack app at api.slack.com/apps, install it to your workspace, and grant it the right bot scopes. Once connected, Claude can draft responses, triage support channels, or post automated summaries.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Have Claude monitor a support channel and draft responses to common questions.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-slack</code>
                <div class="card-takeaway"><strong>Key takeaway:</strong> Use this to bring Claude into team communication workflows — reading channels and drafting messages without copy-pasting.</div>
```

- [ ] **Step 7: Replace card 07 body (Fetch)**

Find:
```html
                Fetches raw HTML or text from any public URL without opening a browser. Useful for pulling in documentation pages, RSS feeds, or plain-text files directly into your conversation.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Point Claude at an API docs page and ask it to write a client based on what it reads.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-fetch</code>
```

Replace with:
```html
                Fetches raw HTML or plain text from any public URL — no browser, no JavaScript execution. It's the lightest-weight way to pull in external content: documentation pages, API responses, README files, RSS feeds, or any server-rendered page. Unlike Puppeteer, it doesn't run JavaScript, so it's fast and simple but only works on pages that render content server-side. No API key required. Pair it with Brave Search for a full research workflow: search returns the URLs, Fetch retrieves the full text.<br><br>
                <strong style="color:var(--text-mid)">Classroom use:</strong> Point Claude at an API docs page and ask it to write a client based on what it reads.<br><br>
                <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">npx -y @modelcontextprotocol/server-fetch</code>
                <div class="card-takeaway"><strong>Key takeaway:</strong> Use Fetch for static and server-rendered pages; switch to Puppeteer if the page needs JavaScript to load its content.</div>
```

- [ ] **Step 8: Verify in browser**

Open `index.html`, scroll to Section 4. Expand each of the 7 cards and confirm: expanded text is present, API key references appear on GitHub, Brave, and Slack cards, and all 7 takeaway blocks render correctly.

- [ ] **Step 9: Commit**

```bash
git add index.html
git commit -m "feat: expand Section 4 cards and add key takeaways"
```

---

## Task 6: Section 5 — Use Cases (5 cards)

**Files:**
- Modify: `index.html` — card bodies inside `#use-cases`

- [ ] **Step 1: Replace card 01 body (Research Assistant)**

Find:
```html
                <strong style="color:var(--text-mid)">Problem:</strong> You need to research a topic across multiple sources and synthesize findings without copy-pasting from tab to tab.<br><br>
                <strong style="color:var(--text-mid)">Servers:</strong> Brave Search + Fetch<br><br>
                <strong style="color:var(--text-mid)">Outcome:</strong> Claude searches the web for your query, fetches the full text of the top results, and returns a structured summary with citations — all in one response.
```

Replace with:
```html
                <strong style="color:var(--text-mid)">Problem:</strong> You need to research a topic across multiple sources and synthesize findings without copy-pasting from tab to tab.<br><br>
                <strong style="color:var(--text-mid)">Servers:</strong> Brave Search + Fetch<br><br>
                <strong style="color:var(--text-mid)">Outcome:</strong> Claude runs the search, fetches the full text of the top 3–5 results, cross-references them, and returns a structured summary with source citations — all in a single response. What normally takes 20 minutes of tab-switching takes under 30 seconds. The citations are real URLs you can verify yourself.
                <div class="card-takeaway"><strong>Key takeaway:</strong> The pattern: search → fetch → synthesize. One prompt replaces an entire manual research session.</div>
```

- [ ] **Step 2: Replace card 02 body (Code Reviewer)**

Find:
```html
                <strong style="color:var(--text-mid)">Problem:</strong> You want AI feedback on a pull request without pasting the whole diff into a chat window.<br><br>
                <strong style="color:var(--text-mid)">Server:</strong> GitHub<br><br>
                <strong style="color:var(--text-mid)">Outcome:</strong> Claude fetches the PR diff directly from GitHub, reads the changed files, and returns structured review comments organized by file — pointing to specific lines.
```

Replace with:
```html
                <strong style="color:var(--text-mid)">Problem:</strong> You want AI feedback on a pull request without pasting the whole diff into a chat window.<br><br>
                <strong style="color:var(--text-mid)">Server:</strong> GitHub<br><br>
                <strong style="color:var(--text-mid)">Outcome:</strong> Claude fetches the PR diff directly from GitHub, reads the changed files and their surrounding context, and returns structured review comments organized by file — pointing to specific lines. You get the same output as a human code review: what changed, what could break, what edge cases weren't handled. Because it reads the actual diff, the feedback references real line numbers, not paraphrases.
                <div class="card-takeaway"><strong>Key takeaway:</strong> The pattern: fetch diff → analyze changes → structured output. Claude reviews your PR the same way a teammate would.</div>
```

- [ ] **Step 3: Replace card 03 body (Database Explorer)**

Find:
```html
                <strong style="color:var(--text-mid)">Problem:</strong> A teammate handed you a Postgres database with no documentation and you need to understand it fast.<br><br>
                <strong style="color:var(--text-mid)">Server:</strong> Postgres<br><br>
                <strong style="color:var(--text-mid)">Outcome:</strong> Claude queries the schema, describes each table in plain English, maps out the relationships, and answers questions like "which table tracks user sessions?" without you writing a single query.
```

Replace with:
```html
                <strong style="color:var(--text-mid)">Problem:</strong> A teammate handed you a Postgres database with no documentation and you need to understand it fast.<br><br>
                <strong style="color:var(--text-mid)">Server:</strong> Postgres<br><br>
                <strong style="color:var(--text-mid)">Outcome:</strong> Claude queries the schema, describes each table in plain English, and maps out the relationships between them. Because it loaded the full schema in its first query, you can then ask follow-up questions like "which table tracks user sessions?" or "write a query that joins orders to users" and it already has the context to answer accurately — no copy-pasting schema dumps required.
                <div class="card-takeaway"><strong>Key takeaway:</strong> The pattern: read schema → answer questions in plain English. No documentation needed to understand an unfamiliar database.</div>
```

- [ ] **Step 4: Replace card 04 body (Web Scraper)**

Find:
```html
                <strong style="color:var(--text-mid)">Problem:</strong> You need structured data from a website that doesn't offer an API.<br><br>
                <strong style="color:var(--text-mid)">Server:</strong> Puppeteer<br><br>
                <strong style="color:var(--text-mid)">Outcome:</strong> Claude navigates to the page, waits for dynamic content to load, extracts the data you describe (prices, names, dates), and returns it as a clean table ready to copy into a spreadsheet.
```

Replace with:
```html
                <strong style="color:var(--text-mid)">Problem:</strong> You need structured data from a website that doesn't offer an API.<br><br>
                <strong style="color:var(--text-mid)">Server:</strong> Puppeteer<br><br>
                <strong style="color:var(--text-mid)">Outcome:</strong> Claude navigates to the page, waits for JavaScript to render, extracts the specific data you described (prices, names, dates, links), and returns it as a clean structured table. You can ask it to handle pagination, filter by criteria, or scrape multiple pages — all in plain English. The output is ready to paste into a spreadsheet or feed into another script.
                <div class="card-takeaway"><strong>Key takeaway:</strong> The pattern: navigate → wait → extract → structure. Describe the data you want in plain English; Claude handles the scraping logic.</div>
```

- [ ] **Step 5: Replace card 05 body (Team Automator)**

Find:
```html
                <strong style="color:var(--text-mid)">Problem:</strong> New GitHub issues pile up and it takes time to triage them, assign labels, and notify the team.<br><br>
                <strong style="color:var(--text-mid)">Servers:</strong> GitHub + Slack<br><br>
                <strong style="color:var(--text-mid)">Outcome:</strong> Claude reads open issues, categorizes them by type (bug, feature, question), posts a triage summary to a Slack channel, and suggests label assignments — in one prompt.
```

Replace with:
```html
                <strong style="color:var(--text-mid)">Problem:</strong> New GitHub issues pile up and it takes time to triage them, assign labels, and notify the team.<br><br>
                <strong style="color:var(--text-mid)">Servers:</strong> GitHub + Slack<br><br>
                <strong style="color:var(--text-mid)">Outcome:</strong> Claude reads every open issue, categorizes them by type (bug, feature request, question, docs), estimates priority based on the description and reactions, posts a formatted triage summary to a Slack channel, and suggests label assignments — all from a single prompt. What normally takes a 15-minute triage meeting becomes a one-liner. You review the suggestions, apply what looks right, and move on.
                <div class="card-takeaway"><strong>Key takeaway:</strong> The pattern: read → categorize → act across systems. Combining two servers turns a manual recurring workflow into a single prompt.</div>
```

- [ ] **Step 6: Verify in browser**

Open `index.html`, scroll to Section 5, expand all five cards. Confirm the Problem/Servers/Outcome structure is intact on each, outcomes are expanded, and all five takeaway blocks appear.

- [ ] **Step 7: Commit**

```bash
git add index.html
git commit -m "feat: expand Section 5 cards and add key takeaways"
```

---

## Task 7: Section 6 — Getting Started

**Files:**
- Modify: `index.html` — steps inside `#getting-started`, plus new step and section takeaway

This task has the most structural change: a new step is inserted between current steps 3 and 4, which means steps 4 and 5 must be renumbered to 5 and 6. Do it in order to avoid duplicate numbers mid-edit.

- [ ] **Step 1: Expand Step 4 description (Restart)**

Find:
```html
              <div class="step-num">4</div>
              <div class="step-content">
                <div class="step-title">Restart Claude Desktop</div>
                <div class="step-desc">Fully quit and relaunch Claude Desktop. The config file is only read on startup — changes won't take effect until you restart.</div>
              </div>
```

Replace with:
```html
              <div class="step-num">5</div>
              <div class="step-content">
                <div class="step-title">Restart Claude Desktop</div>
                <div class="step-desc">Fully quit and relaunch Claude Desktop — don't just close the window. On macOS, use Cmd+Q or right-click the dock icon and choose Quit. On Windows, right-click the system tray icon and choose Exit. The config file is only read on startup, so changes won't take effect until you do a full restart.</div>
              </div>
```

- [ ] **Step 2: Expand Step 5 description (Verify) and renumber**

Find:
```html
              <div class="step-num">5</div>
              <div class="step-content">
                <div class="step-title">Verify it's working</div>
                <div class="step-desc">Open a new conversation and type: <em>"List the files in my Documents folder."</em> Claude should call the <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">list_directory</code> tool and return the results. You'll see a tool-use indicator appear in the chat.</div>
              </div>
```

Replace with:
```html
              <div class="step-num">6</div>
              <div class="step-content">
                <div class="step-title">Verify it's working</div>
                <div class="step-desc">Open a new conversation and type: <em>"List the files in my Documents folder."</em> Claude should call the <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">list_directory</code> tool and return the results. You'll see a small tool-use indicator — a collapsible row showing the tool name and its inputs — appear above Claude's response. If nothing happens or you get an error, double-check that the config file path is correct and that you fully quit and relaunched (not just closed the window).</div>
              </div>
```

- [ ] **Step 3: Insert the new API Keys step between steps 3 and 5**

Find the closing `</div>` of step 3 (the "Add a server entry" step). The step ends with:

```html
                </div>
              </div>
            </div>
            <div class="step">
              <div class="step-num">5</div>
              <div class="step-content">
                <div class="step-title">Restart Claude Desktop</div>
```

Replace that transition with:

```html
                </div>
              </div>
            </div>
            <div class="step">
              <div class="step-num">4</div>
              <div class="step-content">
                <div class="step-title">Handle API keys</div>
                <div class="step-desc">
                  Some MCP servers need an API key to authenticate with an external service. Pass keys using the <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">env</code> field in your server config — never hardcode them inside <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">args</code>.<br><br>
                  <strong style="color:var(--text-mid)">Config pattern with an API key</strong>
                  <code class="code-block">{
  "mcpServers": {
    "brave-search": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-brave-search"],
      "env": {
        "BRAVE_API_KEY": "your-key-here"
      }
    }
  }
}</code>
                  <strong style="color:var(--text-mid)">Where to get keys for common servers</strong><br>
                  <strong>GitHub</strong> — github.com → Settings → Developer settings → Personal access tokens → Tokens (classic). Select the <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">repo</code> scope.<br>
                  <strong>Brave Search</strong> — brave.com/search/api — free tier includes 2,000 queries/month.<br>
                  <strong>Slack</strong> — api.slack.com/apps — create an app, install it to your workspace, copy the Bot User OAuth Token.<br><br>
                  <strong style="color:var(--text-mid)">Important:</strong> your config file contains your API keys in plain text. Never commit it to git. Add the config path to your <code style="font-family:var(--font-code);color:var(--accent);font-size:11px">.gitignore</code> or keep keys in a separate secrets file.
                </div>
              </div>
            </div>
            <div class="step">
              <div class="step-num">5</div>
              <div class="step-content">
                <div class="step-title">Restart Claude Desktop</div>
```

- [ ] **Step 4: Add section-level takeaway after the steps div**

Find the closing `</div>` that ends the `.steps` container, immediately before `</section>` in Section 6:

```html
          </div>
        </section>

        <hr class="section-divider" />

        <!-- ── Section 7: Code Examples ── -->
```

Replace with:

```html
          </div>
          <div class="section-takeaway"><strong>Key takeaway:</strong> Once you've done this once, adding new servers is just one config block and a restart — the pattern is always the same.</div>
        </section>

        <hr class="section-divider" />

        <!-- ── Section 7: Code Examples ── -->
```

- [ ] **Step 5: Verify in browser**

Open `index.html`, scroll to Section 6. Confirm: 6 steps are visible (numbered 1–6), Step 4 is "Handle API keys" with the config block and the three bullet links, the section-level takeaway appears below all steps with a blue left border.

- [ ] **Step 6: Commit**

```bash
git add index.html
git commit -m "feat: expand Section 6 steps, add API Keys step, add section takeaway"
```

---

## Task 8: Section 7 — Code Examples

**Files:**
- Modify: `index.html` — section description and post-code takeaway inside `#code-examples`

- [ ] **Step 1: Replace the section description**

Find:
```html
          <div class="section-desc">
            A minimal MCP server with one tool: <code style="font-family:var(--font-code);color:var(--accent);font-size:12px">search_web(query)</code>. It calls the Brave Search API and returns the top five results. Pick your language — the structure is identical in both.
          </div>
```

Replace with:
```html
          <div class="section-desc">
            A minimal MCP server with one tool: <code style="font-family:var(--font-code);color:var(--accent);font-size:12px">search_web(query)</code>. It calls the Brave Search API and returns the top five results. Pick your language — the structure is identical in both. Every MCP server follows three steps: <strong>(1)</strong> instantiate a server with a name, <strong>(2)</strong> register one or more tools with a name, description, and input schema, <strong>(3)</strong> connect a transport and run. The description you write for a tool is what the model reads to decide when to call it — write it like a clear function comment.
          </div>
```

- [ ] **Step 2: Add section-level takeaway after the tab group**

Find the closing `</div>` that ends the `.tab-group` div, before `</section>` in Section 7:

```html
          </div>
        </section>

      </div>
    </main>
```

Replace with:

```html
          </div>
          <div class="section-takeaway"><strong>Key takeaway:</strong> Every MCP server follows the same three-part structure — instantiate, register tools, connect transport. Swap in different tool logic and it works for any use case.</div>
        </section>

      </div>
    </main>
```

- [ ] **Step 3: Verify in browser**

Open `index.html`, scroll to Section 7. Confirm: section description now includes the three numbered steps explanation, both language tabs still work (Python / TypeScript), section takeaway appears below the code block with a blue left border.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: expand Section 7 description and add section takeaway"
```

---

## Done

All 8 tasks complete. The site now has:
- Expanded card bodies across all 7 sections (3–5 sentences each)
- `.card-takeaway` block on every accordion card
- `.section-takeaway` at the end of Sections 6 and 7
- New API Keys step (Step 4) in Section 6 with config example and key sources
- Steps 4 and 5 in Section 6 renumbered to 5 and 6
