# MCP Servers — Presentation Script

> ~15 minutes | Read at a comfortable pace, pause at bullet points

---

## INTRO

_(pull up the site, let it sit on screen)_

Hey everyone. Today I'm going to show you something that genuinely changed the way I use AI in my projects, and by the end of this you'll have it running on your own machine.

It's called MCP — Model Context Protocol. And once you understand it, you'll wonder how you were getting by without it.

---

## SECTION 1: What is MCP?

So let's start from the beginning. What even is this thing?

- MCP stands for Model Context Protocol. It's an open standard that Anthropic released in late 2024.
- The simplest way to think about it is this: AI models like Claude are incredibly smart, but out of the box they can only work with text you paste into the chat window.
- MCP changes that. It gives the model a way to actually _do_ things — read your files, query a database, browse the web, post to Slack.
- The tagline on the site is "USB-C for AI integrations" and I think that nails it. One standard interface, works everywhere.

**Card 1 — Why does MCP exist?**

- Before MCP, every integration was a one-off custom build. You want Claude to read your files? Custom code. Query your database? More custom code, completely separate.
- MCP standardizes all of that. You write a server once and it works with any MCP-compatible client — Claude Desktop, Cursor, VS Code, all of them.

**Card 2 — How is it different from a REST API?**

- With a regular REST API, _you_ decide what to call and when. Your code triggers it.
- With MCP, the model decides. The user types something, the model reads what tools are available, and it figures out which one to use on its own.
- Think of REST as a menu where the developer orders. MCP is a menu where the AI orders.

**Card 3 — Who made it and who supports it?**

- Anthropic built it and open-sourced it in late 2024.
- It's already been adopted by Cursor, VS Code Copilot, Zed, Continue — basically every major AI-powered editor.
- Official SDKs for Python and TypeScript. That's it. That's all you need.

---

## SECTION 2: How it Works

Alright, so how does this actually work under the hood?

- MCP uses a client-server architecture. There are three layers: the Host, the Client, and the Server.

**Card 1 — The three layers**

- The **Host** is the app the user is running — Claude Desktop, Cursor, whatever. You don't build this; you just use it.
- The **Client** lives inside the host and manages the connection to your server. Also handled for you.
- The **Server** is what _you_ build. It exposes tools the model can call. It can be 30 lines of Python. That's genuinely all it takes.

**Card 2 — Transport types: stdio vs HTTP+SSE**

- There are two ways the host talks to your server.
- **stdio** means it launches your server as a child process right on your machine. No networking, no ports, no setup. This is what you'll use day to day.
- **HTTP + SSE** is for when you need the server running remotely — deployed to a server, shared across a team. You don't need to worry about this yet. Start with stdio.

**Card 3 — The request flow**

- Here's what actually happens when you send a message:
  1. You type something in the chat
  2. The model checks what tools are available from all connected servers
  3. It decides to use one and fires off a structured request
  4. Your server runs the tool and returns a result
  5. That result gets injected back into the model's context
  6. The model writes its final response using that result
- Your server handles exactly one step in that chain. Receive a call, return a result. The rest is handled for you.

---

## SECTION 3: Core Capabilities

MCP servers expose three types of things. Let's go through them quickly.

**Card 1 — Tools**

- A tool is a function your server exposes. It has a name, a description, and a defined set of inputs.
- The model reads the description to decide when to use it, then calls it with the right arguments.
- In Python you literally just put `@mcp.tool()` on a function and the SDK handles the rest.
- About 90% of what you'll ever build with MCP is just tools. Everything else is optional.

**Card 2 — Resources**

- Resources are read-only data sources identified by a URI — like `file:///path/to/doc.txt` or `db://users/42`.
- The model fetches them directly instead of calling a function. No arguments, just an ID.
- Think config files, database rows, documentation. Good fit when your content has a stable ID and doesn't need any logic to retrieve.

**Card 3 — Prompts**

- Servers can also expose reusable prompt templates. The host surfaces them as slash commands.
- They're the least common of the three primitives. Most servers never define any.
- Get tools working first. Come back to prompts if you find yourself repeating the same conversation setup over and over.

---

## SECTION 4: Popular Servers

Okay, now let's look at actual servers you can use today. The MCP ecosystem has hundreds of them. I've picked seven that are genuinely useful and each installs with a single command.

**Card 1 — Filesystem**

- Start here. Give it a folder path and Claude gets full read/write access to everything inside it.
- It's sandboxed to that folder, no API key needed, and it's the easiest way to confirm your whole MCP setup is working.

**Card 2 — GitHub**

- Read repos, list PRs, open issues, post comments, browse commit history — all without leaving Claude.
- Works on any repo you have access to, public or private. Needs a GitHub personal access token.
- Once you have this set up, you can ask Claude to review a PR or explain a commit in seconds.

**Card 3 — Postgres**

- Point it at a database with a connection string and Claude can run read-only queries and explain your schema in plain English.
- You don't write any SQL. You just ask questions.

**Card 4 — Brave Search**

- Live web search. This is how you get Claude past its training cutoff.
- Pair it with Fetch and you've got a full research pipeline: search finds URLs, Fetch reads the full pages, Claude synthesizes everything.

**Card 5 — Puppeteer**

- Full headless Chrome. Claude can navigate pages, click buttons, wait for JavaScript to load, take screenshots, extract data.
- If a page renders with JavaScript, plain Fetch won't work. This is what you need instead.

**Card 6 — Slack**

- Send and read messages, list channels, post formatted content to any workspace.
- Needs a Slack bot token, but once it's set up Claude can draft responses, triage channels, or post automated summaries.

**Card 7 — Fetch**

- The lightweight option. Pulls raw HTML or plain text from any public URL — no browser, no JavaScript execution.
- Great for static pages and docs. Fast and zero config.

---

## SECTION 5: Use Cases

Here's where it gets really interesting. These servers are most powerful when you combine them. Let me walk through a few real scenarios.

**Card 1 — Research Assistant**

- Problem: you need to research something across multiple sources without copy-pasting between tabs.
- You connect Brave Search and Fetch.
- Claude searches, fetches the top results, cross-references them, and hands you a structured summary with citations. What used to take 20 minutes takes under 30 seconds.

**Card 2 — Code Reviewer**

- Problem: you want AI feedback on a pull request without pasting the whole diff into chat.
- You connect GitHub.
- Claude pulls the diff directly, reads the changed files in context, and gives you review comments by file and line number. Same output as a human reviewer.

**Card 3 — Database Explorer**

- Problem: a teammate handed you a Postgres database with no docs and you need to understand it fast.
- You connect Postgres.
- Claude reads the schema, explains every table in plain English, maps the relationships. You can then ask follow-up questions and it already has the full context loaded.

**Card 4 — Web Scraper**

- Problem: you need structured data from a site that has no API.
- You connect Puppeteer.
- Describe what you want in plain English. Claude navigates to the page, waits for everything to load, extracts the data, and hands it back as a clean table.

**Card 5 — Team Automator**

- Problem: GitHub issues pile up and triaging them manually is a chore.
- You connect GitHub and Slack.
- One prompt: Claude reads every open issue, categorizes them, estimates priority, posts a triage summary to Slack, and suggests labels. What used to be a 15-minute meeting becomes a one-liner.

---

## SECTION 6: Getting Started

Alright, let's actually do this. We're using Claude CLI as our setup tool — it's faster than Claude Desktop because there's no config file to hunt down. I'll mention how Desktop fits in too so you know both options.

1. **Install Claude CLI** — run `npm install -g @anthropic-ai/claude-code`, then type `claude` and follow the prompts to log in. That's your host — it's what manages the MCP connections.

2. **Add the Filesystem server** — one command: `claude mcp add filesystem npx -y @modelcontextprotocol/server-filesystem ~/Documents`. No config file, no restart. It's live immediately.

3. **Handle API keys** — some servers need one. Use the `-e` flag to pass them: `claude mcp add brave-search -e BRAVE_API_KEY=your-key npx -y @modelcontextprotocol/server-brave-search`. The site shows exactly where to get keys for GitHub, Brave, and Slack.

4. **Verify it's working** — run `claude mcp list` to confirm the server is connected, then start a chat with `claude` and type "List the files in my Documents folder." You'll see the tool call appear inline before the response.

_(pause)_ That's four steps and it's done. Adding more servers later is just one more `claude mcp add` command.

If you prefer a GUI over the terminal, Claude Desktop supports the same servers. The config lives in a JSON file instead, and the format is identical — just a different interface for the same thing.

---

## SECTION 7: Code Examples

Last section — let's look at what a server actually looks like in code.

- The site has a minimal example: a server that exposes one tool called `search_web` that calls the Brave Search API.
- Every MCP server follows the same three steps:
  1. Instantiate the server with a name
  2. Register your tools — give each one a name, description, and input schema
  3. Connect a transport and run

- The description you write for a tool is what the model reads to decide when to call it. Write it like a clear function comment.
- The Python and TypeScript versions are on the site. The structure is identical in both.
- The site has the install command right above the code. Copy it, run it, swap in your logic, and you've got a working MCP server.

---

## WRAP UP

So to recap what we covered:

- MCP is the standard that lets AI models actually take actions, not just talk
- You build the server, the host app handles everything else
- There are hundreds of existing servers you can drop in today
- Combining servers is where the real power is
- Getting started is genuinely six steps — and step one is just downloading an app

The site is yours to keep. All the install commands are there, the API key instructions are there, and the code examples are there to copy.

If you get stuck, the spec lives at github.com/modelcontextprotocol — it's readable and well-documented.

Any questions?

---

> **Timing guide:** Intro ~1 min | Sections 1–3 ~4 min | Section 4 ~3 min | Section 5 ~2.5 min | Section 6 ~2.5 min | Section 7 ~1.5 min | Wrap up ~1 min = ~15.5 min
