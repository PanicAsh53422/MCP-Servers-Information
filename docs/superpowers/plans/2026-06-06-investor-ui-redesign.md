# Investor UI Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Strip the current sidebar/accordion UI and rebuild as a full-width, animated investor presentation site with a canvas particle background, scroll-triggered reveals, and a full-screen slideshow presentation mode.

**Architecture:** Single `index.html` file rewrite. All CSS, HTML structure, and JS are replaced. The existing textual content (section descriptions, card body text, code examples) is preserved verbatim — only the presentation layer changes. The file has three logical blocks: `<style>` (CSS), `<body>` (HTML), and `<script>` (JS for canvas, scroll animations, and presentation mode).

**Tech Stack:** Vanilla HTML/CSS/JS, Google Fonts (Inter, Fira Code), Canvas API for particle background, IntersectionObserver for scroll reveals, CSS transitions for presentation mode.

---

### Task 1: CSS Foundation — Reset, Variables, Typography, and Base Layout

**Files:**
- Modify: `index.html:1-1144` (replace entire `<style>` block)

- [ ] **Step 1: Replace the `<style>` block with new CSS foundation**

Delete everything between `<style>` (line 10) and `</style>` (line 1143) and replace with the new CSS. This includes:
- CSS reset and root variables matching the spec color palette
- Body with deep navy gradient background
- Canvas positioning (full-page, z-index 0)
- Floating top nav bar (fixed, semi-transparent, backdrop-blur)
- Full-width content container (max-width 1200px, centered)
- Section spacing, hero layout, responsive breakpoints
- Glass-morphism card surfaces
- Scroll-reveal animation classes
- Presentation mode styles
- Code block and tab styling

```css
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }

:root {
  --bg-start: #020617;
  --bg-end: #0f172a;
  --accent: #38bdf8;
  --accent-2: #a855f7;
  --text-primary: #f1f5f9;
  --text-secondary: rgba(255,255,255,0.5);
  --text-dim: rgba(255,255,255,0.35);
  --surface: rgba(15,23,42,0.6);
  --border: rgba(56,189,248,0.1);
  --border-hover: rgba(56,189,248,0.25);
  --font-ui: 'Inter', system-ui, -apple-system, sans-serif;
  --font-code: 'Fira Code', ui-monospace, SFMono-Regular, Menlo, Consolas, monospace;
}

body {
  min-height: 100vh;
  font-family: var(--font-ui);
  font-size: 16px;
  line-height: 1.65;
  color: var(--text-primary);
  background: linear-gradient(180deg, var(--bg-start) 0%, var(--bg-end) 100%);
  overflow-x: hidden;
}

code { font-family: var(--font-code); }

/* ── Canvas background ── */
#particle-canvas {
  position: fixed;
  inset: 0;
  z-index: 0;
  pointer-events: none;
}

/* ── Top navigation ── */
.top-nav {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 64px;
  padding: 0 32px;
  background: rgba(2,6,23,0.7);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border-bottom: 1px solid var(--border);
  transition: transform 0.4s ease;
}

.top-nav.hidden {
  transform: translateY(-100%);
}

.nav-brand {
  font-size: 13px;
  font-weight: 600;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: var(--text-secondary);
}

.nav-brand span {
  color: var(--accent);
}

.nav-links {
  display: flex;
  align-items: center;
  gap: 8px;
  list-style: none;
}

.nav-links a {
  position: relative;
  padding: 6px 14px;
  color: var(--text-secondary);
  font-size: 13px;
  font-weight: 500;
  text-decoration: none;
  border-radius: 6px;
  transition: color 0.3s ease, background 0.3s ease;
}

.nav-links a:hover {
  color: var(--text-primary);
  background: rgba(255,255,255,0.05);
}

.nav-links a.active {
  color: var(--accent);
}

.nav-links a.active::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 14px;
  right: 14px;
  height: 2px;
  background: var(--accent);
  border-radius: 1px;
}

.present-btn {
  display: flex;
  align-items: center;
  gap: 6px;
  margin-left: 16px;
  padding: 7px 16px;
  border: 1px solid var(--border);
  border-radius: 6px;
  background: transparent;
  color: var(--text-secondary);
  font-family: var(--font-ui);
  font-size: 12px;
  font-weight: 600;
  letter-spacing: 0.5px;
  cursor: pointer;
  transition: border-color 0.3s ease, color 0.3s ease, background 0.3s ease;
}

.present-btn:hover {
  border-color: var(--accent);
  color: var(--accent);
  background: rgba(56,189,248,0.05);
}

/* ── Main content wrapper ── */
.site-content {
  position: relative;
  z-index: 1;
}

.content-wrap {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 48px;
}

/* ── Hero section ── */
.hero {
  display: flex;
  align-items: center;
  justify-content: space-between;
  min-height: 100vh;
  padding-top: 64px;
  gap: 48px;
}

.hero-text {
  flex: 1;
  max-width: 600px;
}

.hero-eyebrow {
  margin-bottom: 24px;
  font-size: 12px;
  font-weight: 700;
  letter-spacing: 3px;
  text-transform: uppercase;
  color: rgba(56,189,248,0.7);
}

.hero-title {
  font-size: clamp(42px, 5.5vw, 64px);
  font-weight: 800;
  line-height: 1.05;
  letter-spacing: -1.5px;
  margin-bottom: 24px;
}

.hero-title .gradient-text {
  background: linear-gradient(135deg, var(--accent), var(--accent-2));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.hero-desc {
  max-width: 480px;
  font-size: 17px;
  line-height: 1.7;
  color: var(--text-dim);
  margin-bottom: 36px;
}

.hero-actions {
  display: flex;
  gap: 14px;
}

.btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 12px 28px;
  border-radius: 8px;
  font-family: var(--font-ui);
  font-size: 14px;
  font-weight: 600;
  letter-spacing: 0.5px;
  text-decoration: none;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.btn:hover {
  transform: translateY(-1px);
}

.btn-outline {
  border: 1px solid var(--border-hover);
  background: transparent;
  color: var(--accent);
}

.btn-outline:hover {
  border-color: var(--accent);
  box-shadow: 0 0 20px rgba(56,189,248,0.1);
}

.btn-gradient {
  border: none;
  background: linear-gradient(135deg, var(--accent), var(--accent-2));
  color: #fff;
}

.btn-gradient:hover {
  box-shadow: 0 4px 24px rgba(56,189,248,0.3);
}

/* ── Hero orbital graphic ── */
.hero-graphic {
  flex: 0 0 400px;
  height: 400px;
  position: relative;
}

.orbital-container {
  position: relative;
  width: 100%;
  height: 100%;
}

.orbit-ring {
  position: absolute;
  border: 1px solid;
  border-radius: 50%;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.orbit-ring-1 {
  width: 160px;
  height: 160px;
  border-color: rgba(56,189,248,0.15);
}

.orbit-ring-2 {
  width: 260px;
  height: 260px;
  border-color: rgba(168,85,247,0.12);
}

.orbit-ring-3 {
  width: 360px;
  height: 360px;
  border-color: rgba(56,189,248,0.08);
}

.orbit-center {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(56,189,248,0.2) 0%, rgba(168,85,247,0.1) 60%, transparent 100%);
  box-shadow: 0 0 40px rgba(56,189,248,0.15);
}

.orbit-node {
  position: absolute;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  top: 50%;
  left: 50%;
}

.orbit-node-label {
  position: absolute;
  white-space: nowrap;
  font-size: 10px;
  font-weight: 600;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: var(--text-dim);
  pointer-events: none;
}

.orbit-pulse {
  position: absolute;
  width: 4px;
  height: 4px;
  border-radius: 50%;
  background: var(--accent);
  box-shadow: 0 0 8px var(--accent);
  opacity: 0;
}

/* ── Content sections ── */
.section {
  padding: 120px 0;
  scroll-margin-top: 80px;
}

.section-header {
  margin-bottom: 56px;
}

.section-eyebrow {
  display: block;
  margin-bottom: 16px;
  font-size: 12px;
  font-weight: 700;
  letter-spacing: 3px;
  text-transform: uppercase;
  color: var(--accent);
  opacity: 0.7;
}

.section-title {
  font-size: clamp(36px, 5vw, 56px);
  font-weight: 800;
  line-height: 1;
  letter-spacing: -1px;
  margin-bottom: 20px;
}

.section-title .gradient-text {
  background: linear-gradient(135deg, var(--accent), var(--accent-2));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.section-desc {
  max-width: 720px;
  font-size: 17px;
  line-height: 1.75;
  color: var(--text-secondary);
}

/* ── Glass cards ── */
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(340px, 1fr));
  gap: 20px;
}

.card-grid.single-col {
  grid-template-columns: 1fr;
}

.glass-card {
  padding: 28px;
  border: 1px solid var(--border);
  border-radius: 12px;
  background: var(--surface);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  transition: border-color 0.3s ease, transform 0.3s ease, box-shadow 0.3s ease;
}

.glass-card:hover {
  border-color: var(--border-hover);
  transform: translateY(-2px);
  box-shadow: 0 16px 48px rgba(0,0,0,0.2);
}

.card-num {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 36px;
  height: 36px;
  margin-bottom: 16px;
  border: 1px solid rgba(56,189,248,0.2);
  border-radius: 8px;
  background: rgba(56,189,248,0.06);
  color: var(--accent);
  font-family: var(--font-code);
  font-size: 12px;
  font-weight: 700;
}

.card-title {
  font-size: 18px;
  font-weight: 700;
  margin-bottom: 12px;
  color: var(--text-primary);
}

.card-body-text {
  color: var(--text-secondary);
  font-size: 15px;
  line-height: 1.75;
}

.card-body-text strong {
  color: var(--text-primary);
}

.card-body-text a {
  color: var(--accent);
  text-decoration: underline;
  text-underline-offset: 3px;
}

.card-takeaway {
  margin-top: 16px;
  padding: 12px 16px;
  border-left: 2px solid var(--accent);
  border-radius: 0 6px 6px 0;
  background: rgba(56,189,248,0.04);
  font-size: 14px;
  color: var(--text-secondary);
  line-height: 1.7;
}

.card-takeaway strong {
  color: var(--accent);
}

.section-takeaway {
  margin-top: 32px;
  padding: 16px 20px;
  border-left: 2px solid var(--accent-2);
  border-radius: 0 6px 6px 0;
  background: rgba(168,85,247,0.04);
  font-size: 15px;
  color: var(--text-secondary);
  line-height: 1.7;
}

.section-takeaway strong {
  color: var(--accent-2);
}

/* ── Steps (Getting Started) ── */
.steps-grid {
  display: grid;
  gap: 20px;
}

.step-card {
  display: flex;
  gap: 20px;
  padding: 24px;
  border: 1px solid var(--border);
  border-radius: 12px;
  background: var(--surface);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  transition: border-color 0.3s ease, transform 0.3s ease;
}

.step-card:hover {
  border-color: var(--border-hover);
  transform: translateY(-1px);
}

.step-num {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 42px;
  height: 42px;
  border: 1px solid rgba(56,189,248,0.2);
  border-radius: 10px;
  background: rgba(56,189,248,0.06);
  color: var(--accent);
  font-family: var(--font-code);
  font-size: 16px;
  font-weight: 700;
  flex-shrink: 0;
}

.step-content { flex: 1; min-width: 0; }

.step-title {
  font-size: 18px;
  font-weight: 700;
  margin-bottom: 10px;
  color: var(--text-primary);
}

.step-desc {
  color: var(--text-secondary);
  font-size: 15px;
  line-height: 1.75;
}

/* ── Code blocks ── */
.code-block {
  display: block;
  max-width: 100%;
  margin-top: 12px;
  padding: 16px 20px;
  overflow-x: auto;
  border: 1px solid rgba(56,189,248,0.12);
  border-radius: 8px;
  background: rgba(2,6,23,0.8);
  color: #7dd3fc;
  font-family: var(--font-code);
  font-size: 13px;
  line-height: 1.8;
  white-space: pre;
}

.inline-code {
  color: var(--accent);
  font-family: var(--font-code);
  font-size: 0.88em;
  background: rgba(56,189,248,0.08);
  border: 1px solid rgba(56,189,248,0.15);
  border-radius: 5px;
  padding: 1px 6px;
}

/* ── Tabs ── */
.tab-group { margin-top: 8px; }

.tabs {
  display: flex;
  gap: 4px;
  width: fit-content;
  margin-bottom: 16px;
  padding: 4px;
  border: 1px solid var(--border);
  border-radius: 8px;
  background: rgba(2,6,23,0.6);
}

.tab-btn {
  padding: 8px 20px;
  border: none;
  border-radius: 6px;
  background: transparent;
  color: var(--text-secondary);
  font-family: var(--font-code);
  font-size: 13px;
  font-weight: 500;
  cursor: pointer;
  transition: color 0.2s ease, background 0.2s ease;
}

.tab-btn:hover { color: var(--text-primary); }

.tab-btn.active {
  background: rgba(56,189,248,0.12);
  color: var(--text-primary);
}

.tab-panel { display: none; }
.tab-panel.active { display: block; }

.code-block-label {
  margin-bottom: 8px;
  color: var(--text-dim);
  font-family: var(--font-code);
  font-size: 11px;
}

/* ── Section dividers ── */
.section-divider {
  height: 1px;
  border: 0;
  background: linear-gradient(90deg, transparent, var(--border-hover), transparent);
  margin: 0;
}

/* ── Scroll reveal animations ── */
.reveal {
  opacity: 0;
  transform: translateY(35px);
  transition: opacity 0.6s ease-out, transform 0.6s ease-out;
}

.reveal.visible {
  opacity: 1;
  transform: translateY(0);
}

/* ── Hero load animations ── */
.hero-anim {
  opacity: 0;
  transform: translateY(20px);
}

.hero-anim.loaded {
  opacity: 1;
  transform: translateY(0);
  transition: opacity 0.6s ease-out, transform 0.6s ease-out;
}

.hero-graphic-anim {
  opacity: 0;
  transform: scale(0.8);
}

.hero-graphic-anim.loaded {
  opacity: 1;
  transform: scale(1);
  transition: opacity 0.8s ease-out, transform 0.8s ease-out;
}

/* ── Presentation mode ── */
body.presenting {
  overflow: hidden;
}

body.presenting .site-content {
  position: fixed;
  inset: 0;
  z-index: 50;
}

.slide-container {
  display: none;
}

body.presenting .slide-container {
  display: block;
  position: fixed;
  inset: 0;
  z-index: 50;
}

.slide {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 60px 48px;
  opacity: 0;
  transform: scale(1.05);
  transition: opacity 0.5s ease-in-out, transform 0.5s ease-in-out;
  pointer-events: none;
  overflow-y: auto;
}

.slide.active {
  opacity: 1;
  transform: scale(1);
  pointer-events: auto;
}

.slide.exiting {
  opacity: 0;
  transform: scale(0.95);
  transition: opacity 0.4s ease-in-out, transform 0.4s ease-in-out;
}

/* Particle dissolve transition */
.slide.dissolve-out .reveal,
.slide.dissolve-out .glass-card,
.slide.dissolve-out .step-card,
.slide.dissolve-out .section-header,
.slide.dissolve-out .hero-text,
.slide.dissolve-out .hero-graphic {
  transition: opacity 0.5s ease-out, transform 0.5s ease-out;
  opacity: 0;
}

.slide.dissolve-in .reveal,
.slide.dissolve-in .glass-card,
.slide.dissolve-in .step-card,
.slide.dissolve-in .section-header,
.slide.dissolve-in .hero-text,
.slide.dissolve-in .hero-graphic {
  opacity: 0;
  animation: coalesce 0.6s ease-out forwards;
}

@keyframes coalesce {
  from {
    opacity: 0;
    transform: translate(var(--dx, 0), var(--dy, 0)) rotate(var(--dr, 0deg));
  }
  to {
    opacity: 1;
    transform: translate(0, 0) rotate(0deg);
  }
}

/* Presentation progress bar */
.pres-progress {
  display: none;
  position: fixed;
  bottom: 24px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 200;
  gap: 8px;
  align-items: center;
}

body.presenting .pres-progress {
  display: flex;
}

.pres-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255,255,255,0.2);
  transition: background 0.3s ease, transform 0.3s ease;
}

.pres-dot.active {
  background: var(--accent);
  transform: scale(1.3);
}

.pres-counter {
  margin-left: 16px;
  font-size: 13px;
  font-weight: 500;
  color: var(--text-dim);
  font-family: var(--font-code);
}

/* Presentation vignette */
body.presenting::after {
  content: '';
  position: fixed;
  inset: 0;
  z-index: 49;
  pointer-events: none;
  background: radial-gradient(ellipse at center, transparent 60%, rgba(2,6,23,0.4) 100%);
}

/* Cursor auto-hide in presentation */
body.presenting.cursor-hidden,
body.presenting.cursor-hidden * {
  cursor: none !important;
}

/* ── Responsive ── */
@media (max-width: 1024px) {
  .hero {
    flex-direction: column;
    text-align: center;
    padding-top: 100px;
  }

  .hero-text { max-width: 100%; }
  .hero-desc { margin-left: auto; margin-right: auto; }
  .hero-actions { justify-content: center; }
  .hero-graphic { flex: 0 0 300px; height: 300px; }

  .nav-links { display: none; }

  .hamburger {
    display: flex;
    flex-direction: column;
    gap: 5px;
    padding: 8px;
    border: none;
    background: transparent;
    cursor: pointer;
  }

  .hamburger span {
    display: block;
    width: 22px;
    height: 2px;
    background: var(--text-secondary);
    border-radius: 1px;
    transition: transform 0.3s ease, opacity 0.3s ease;
  }

  .mobile-nav {
    display: none;
    position: fixed;
    top: 64px;
    left: 0;
    right: 0;
    z-index: 99;
    padding: 16px 24px 24px;
    background: rgba(2,6,23,0.95);
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
    border-bottom: 1px solid var(--border);
  }

  .mobile-nav.open { display: block; }

  .mobile-nav a {
    display: block;
    padding: 12px 0;
    color: var(--text-secondary);
    font-size: 15px;
    font-weight: 500;
    text-decoration: none;
    border-bottom: 1px solid rgba(255,255,255,0.05);
  }

  .mobile-nav a:hover { color: var(--text-primary); }

  .content-wrap { padding: 0 24px; }
  .card-grid { grid-template-columns: 1fr; }
}

@media (max-width: 768px) {
  .hero-graphic { display: none; }
  .hero { min-height: 80vh; }
  .section { padding: 80px 0; }
  .present-btn { display: none; }
}

@media (min-width: 1025px) {
  .hamburger { display: none; }
  .mobile-nav { display: none !important; }
}
```

- [ ] **Step 2: Verify the style block is intact**

Open `index.html` in a browser. You should see a broken page (old HTML, new CSS) — that's expected. The important thing is no CSS syntax errors in the browser console.

Run: Open browser dev tools console and check for errors.
Expected: No CSS parse errors.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: replace CSS with investor redesign foundation"
```

---

### Task 2: HTML Structure — Top Nav, Hero, and Content Sections

**Files:**
- Modify: `index.html:1146-1615` (replace entire `<body>` HTML, keeping `<script>`)

This task replaces all HTML between `<body>` and `<script>`. All textual content from the old accordion cards is preserved but restructured into always-visible glass cards. The sidebar is removed. A floating top nav and full-viewport hero are added.

- [ ] **Step 1: Replace the body HTML**

Delete everything from `<body>` (line 1146) through the closing `</div><!-- end layout -->` (just before the `<script>` tag around line 1616). Replace with the new HTML structure.

The new HTML should have this structure:

```html
<body>
  <canvas id="particle-canvas"></canvas>

  <!-- Top Navigation -->
  <nav class="top-nav">
    <div class="nav-brand"><span>Dixie Tech</span> · AI Division</div>
    <ul class="nav-links">
      <li><a href="#what-is-mcp" data-section="what-is-mcp">What is MCP</a></li>
      <li><a href="#how-it-works" data-section="how-it-works">How it Works</a></li>
      <li><a href="#core-capabilities" data-section="core-capabilities">Capabilities</a></li>
      <li><a href="#popular-servers" data-section="popular-servers">Servers</a></li>
      <li><a href="#use-cases" data-section="use-cases">Use Cases</a></li>
      <li><a href="#getting-started" data-section="getting-started">Get Started</a></li>
      <li><a href="#code-examples" data-section="code-examples">Code</a></li>
    </ul>
    <button class="present-btn" id="present-btn" title="Present (P)">▶ Present</button>
    <button class="hamburger" id="hamburger" aria-label="Toggle menu">
      <span></span><span></span><span></span>
    </button>
  </nav>

  <!-- Mobile nav -->
  <div class="mobile-nav" id="mobile-nav">
    <a href="#what-is-mcp">What is MCP</a>
    <a href="#how-it-works">How it Works</a>
    <a href="#core-capabilities">Capabilities</a>
    <a href="#popular-servers">Servers</a>
    <a href="#use-cases">Use Cases</a>
    <a href="#getting-started">Get Started</a>
    <a href="#code-examples">Code</a>
  </div>

  <div class="site-content">
    <div class="content-wrap">

      <!-- ── Hero ── -->
      <section class="hero" id="hero">
        <div class="hero-text">
          <div class="hero-eyebrow hero-anim" style="transition-delay:0.3s">DIXIE TECH · AI DIVISION</div>
          <h1 class="hero-title">
            <span class="hero-anim" style="transition-delay:0.5s">We Taught AI</span><br>
            <span class="hero-anim gradient-text" style="transition-delay:0.8s">to Use Tools.</span>
          </h1>
          <p class="hero-desc hero-anim" style="transition-delay:1.0s">An open protocol connecting AI models to every API, database, and service your business runs on.</p>
          <div class="hero-actions hero-anim" style="transition-delay:1.2s">
            <a href="#what-is-mcp" class="btn btn-outline">Explore</a>
            <button class="btn btn-gradient" id="hero-present-btn">Present ▶</button>
          </div>
        </div>
        <div class="hero-graphic hero-graphic-anim" style="transition-delay:0.5s">
          <div class="orbital-container" id="orbital">
            <div class="orbit-ring orbit-ring-1"></div>
            <div class="orbit-ring orbit-ring-2"></div>
            <div class="orbit-ring orbit-ring-3"></div>
            <div class="orbit-center"></div>
          </div>
        </div>
      </section>

      <hr class="section-divider" />

      <!-- ── Section 1: What is MCP ── -->
      <section class="section" id="what-is-mcp">
        <div class="section-header reveal">
          <span class="section-eyebrow">01 / Concepts</span>
          <h2 class="section-title">What is <span class="gradient-text">MCP</span>?</h2>
          <p class="section-desc">Model Context Protocol is an open standard by Anthropic that lets AI models talk to external tools, APIs, and data sources through one unified interface. Think of it as USB-C for AI integrations.</p>
        </div>
        <div class="card-grid">
          <!-- Card 1 -->
          <div class="glass-card reveal">
            <div class="card-num">01</div>
            <div class="card-title">Why does MCP exist?</div>
            <div class="card-body-text">Before MCP, every AI tool integration was a custom one-off build. If you wanted Claude to read your files, you built a custom integration. If you wanted it to query your database, you built another one that shared nothing with the first. MCP standardizes how models discover and invoke tools. A server written once works with any MCP-compatible client: Claude CLI, Claude Desktop, Cursor, VS Code, Continue, Zed, and more. If you're already building software with AI, MCP is the layer that lets your AI actually take actions in your environment, not just talk about them.<div class="card-takeaway"><strong>Key takeaway:</strong> Write the server once; every MCP-compatible AI client can use it without any changes.</div></div>
          </div>
          <!-- Card 2 -->
          <div class="glass-card reveal">
            <div class="card-num">02</div>
            <div class="card-title">How is it different from a REST API?</div>
            <div class="card-body-text">REST APIs are designed for app-to-app communication with a known schema. Your code decides which endpoint to call and when. MCP is designed for AI-to-tool communication: it includes a built-in discovery step where the model queries what tools are available at runtime, then picks which one to call based on the user's request. You don't write the logic that triggers the tool. The model does, based on what the user asked. Your server doesn't need to know anything about the conversation or the model. It just receives a validated tool call and returns a result. Think of REST as a menu where the developer orders; MCP is a menu where the AI orders.<div class="card-takeaway"><strong>Key takeaway:</strong> With REST, your code decides what to call. With MCP, the model decides, based on the user's intent, not a hardcoded condition.</div></div>
          </div>
          <!-- Card 3 -->
          <div class="glass-card reveal">
            <div class="card-num">03</div>
            <div class="card-title">Who created it and who supports it?</div>
            <div class="card-body-text">Created by Anthropic and open-sourced in late 2024. In under a year it became the de facto standard for AI tool integrations, adopted by Cursor, VS Code Copilot Chat, Continue, Zed, and dozens of other clients. Official SDKs exist for Python and TypeScript, both actively maintained and well-documented. The spec is open and versioned at <a href="https://github.com/modelcontextprotocol" target="_blank" rel="noopener">github.com/modelcontextprotocol</a>. You can read exactly how it works, not just how to use it. The editors you already use support it, so any MCP server you build today works across your whole dev environment without extra setup.<div class="card-takeaway"><strong>Key takeaway:</strong> MCP is young but already the standard. The Python and TypeScript SDKs are all you need to get building.</div></div>
          </div>
        </div>
      </section>

      <hr class="section-divider" />

      <!-- ── Section 2: How it Works ── -->
      <section class="section" id="how-it-works">
        <div class="section-header reveal">
          <span class="section-eyebrow">02 / Architecture</span>
          <h2 class="section-title">How it <span class="gradient-text">Works</span></h2>
          <p class="section-desc">MCP uses a client-server architecture. The AI client connects to one or more servers, each exposing capabilities the model can use mid-conversation.</p>
        </div>
        <div class="card-grid">
          <div class="glass-card reveal">
            <div class="card-num">01</div>
            <div class="card-title">The three layers: Host, Client, Server</div>
            <div class="card-body-text"><strong>Host:</strong> the application the user runs (e.g. Claude CLI, Claude Desktop, Cursor). For this class we're using Claude CLI as our primary host. It manages the user session, renders the conversation UI, and holds one or more clients. You don't build this; you use it.<br><br><strong>Client:</strong> lives inside the host. Maintains a 1:1 connection with one MCP server, forwards tool calls from the model to the server, and routes results back. Also handled for you by the host app.<br><br><strong>Server:</strong> a separate process (or remote service) that exposes tools, resources, and prompts. This is what you build. A server can be 30 lines of Python or TypeScript. It runs independently of whatever AI client the user has.<div class="card-takeaway"><strong>Key takeaway:</strong> As a developer, your job is the server. The host and client are handled by whatever AI app the user is already running.</div></div>
          </div>
          <div class="glass-card reveal">
            <div class="card-num">02</div>
            <div class="card-title">Transport types: stdio vs HTTP+SSE</div>
            <div class="card-body-text"><strong>stdio:</strong> the host launches your server as a child process and talks to it through stdin/stdout. Used for local servers on the same machine. Zero networking setup: no ports, no auth, no firewall rules. This is what you'll use for development and most practical work.<br><br><strong>HTTP + SSE:</strong> your server runs as a regular HTTP service. The client connects via Server-Sent Events for streaming responses. Use this when the server needs to run remotely (a deployed API, a shared team tool) or when multiple users need access to the same server at once.<div class="card-takeaway"><strong>Key takeaway:</strong> Start with stdio. It's local, needs no config, and works right away. Switch to HTTP+SSE only when you need remote or shared access.</div></div>
          </div>
          <div class="glass-card reveal">
            <div class="card-num">03</div>
            <div class="card-title">The request flow</div>
            <div class="card-body-text">When a user sends a message, here's what happens under the hood:<br><br>1. User sends a message to the AI.<br>2. Model reads the full list of available tools from all connected servers.<br>3. Model decides a tool call is needed and emits a structured tool-call request.<br>4. Client routes the request to the correct MCP server.<br>5. Server executes the tool and returns a result.<br>6. Result is injected back into the model's context as a new message.<br>7. Model generates a final response using the result.<br><br>Your server only ever handles step 5. It receives a validated call and returns a result. The model, host, and client handle everything else.<div class="card-takeaway"><strong>Key takeaway:</strong> Your server handles one step: receive a tool call, return a result. Everything else is managed by the client and host.</div></div>
          </div>
        </div>
      </section>

      <hr class="section-divider" />

      <!-- ── Section 3: Core Capabilities ── -->
      <section class="section" id="core-capabilities">
        <div class="section-header reveal">
          <span class="section-eyebrow">03 / Primitives</span>
          <h2 class="section-title">Core <span class="gradient-text">Capabilities</span></h2>
          <p class="section-desc">MCP servers expose three primitives. Tools are the most common. Resources and prompts round out the spec for more advanced use cases.</p>
        </div>
        <div class="card-grid">
          <div class="glass-card reveal">
            <div class="card-num">01</div>
            <div class="card-title">Tools: functions the model can call</div>
            <div class="card-body-text">A tool is a named function with a description and a JSON Schema defining its inputs. The model reads the description to decide when to use it, then passes validated arguments. It never guesses at input shapes. Tools can do anything: query a database, call an external API, run a shell command, write a file. In Python, you define a tool by decorating a function with <code class="inline-code">@mcp.tool()</code>. The SDK infers the schema from your type hints automatically. About 90% of MCP server development is writing tools. Resources and prompts are optional extras you can add later.<div class="card-takeaway"><strong>Key takeaway:</strong> Tools are the main primitive. If you're building an MCP server, you're almost certainly building tools.</div></div>
          </div>
          <div class="glass-card reveal">
            <div class="card-num">02</div>
            <div class="card-title">Resources: read-only context the model can access</div>
            <div class="card-body-text">Resources are identified by a URI (e.g. <code class="inline-code">file:///path/to/doc.txt</code> or <code class="inline-code">db://users/42</code>) and return text or binary content. Resources are passive: the model or host fetches them rather than invoking them with arguments. Think of them as read-only data sources: a config file, a database row, a docs page, a code snippet. They make sense when the data has a stable identifier and doesn't need parameters beyond that ID. In practice, tools are far more common because they're more flexible. But resources are a clean fit when your content maps naturally to a URI.<div class="card-takeaway"><strong>Key takeaway:</strong> Resources give the model read access to named data. Use them when your content has a stable URI and doesn't need arguments beyond an ID.</div></div>
          </div>
          <div class="glass-card reveal">
            <div class="card-num">03</div>
            <div class="card-title">Prompts: reusable prompt templates</div>
            <div class="card-body-text">A server can expose named prompt templates with dynamic arguments, like a function that returns a pre-filled conversation starter. The host can surface these as slash-commands or quick actions in the UI (Claude CLI surfaces them when you type <code class="inline-code">/</code>). Prompts make sense when you've got a repeated workflow that always starts the same way: "review this PR", "explain this error log", "summarize this week's issues". They're the least-used of the three primitives. Most servers never define any. Get comfortable building tools first; come back to prompts when you have a concrete repeated workflow to standardize.<div class="card-takeaway"><strong>Key takeaway:</strong> Prompts are optional and uncommon. Master tools first, then revisit these when you have a concrete repeated workflow.</div></div>
          </div>
        </div>
      </section>

      <hr class="section-divider" />

      <!-- ── Section 4: Popular Servers ── -->
      <section class="section" id="popular-servers">
        <div class="section-header reveal">
          <span class="section-eyebrow">04 / Ecosystem</span>
          <h2 class="section-title">Popular <span class="gradient-text">Servers</span></h2>
          <p class="section-desc">The MCP ecosystem has hundreds of servers. These seven are the most useful for classroom work. Each installs with a single command and covers a distinct capability.</p>
        </div>
        <div class="card-grid">
          <div class="glass-card reveal">
            <div class="card-num">01</div>
            <div class="card-title">Filesystem: read and write local files</div>
            <div class="card-body-text">The simplest MCP server and the best one to start with. Give it a directory path in the config and Claude gets read/write access to every file inside it: list directories, read file contents, write new files, search by name or content. It's sandboxed to the folder you specify, so you control exactly what Claude can touch. It uses only stdio and needs no API key, which makes it the easiest way to confirm your MCP setup is working before moving on to more complex servers.<br><br><strong>Classroom use:</strong> Have Claude read your notes folder and summarize what you worked on this week.<br><br><code class="code-block">npx -y @modelcontextprotocol/server-filesystem /path/to/folder</code><div class="card-takeaway"><strong>Key takeaway:</strong> Start here. No API key needed, and getting it working proves your entire setup is correct.</div></div>
          </div>
          <div class="glass-card reveal">
            <div class="card-num">02</div>
            <div class="card-title">GitHub: repos, PRs, issues, and code</div>
            <div class="card-body-text">Search repositories, read file contents, list and create pull requests, open issues, post comments, and browse commit history, all from inside Claude. It works on any repo you have access to, public or private. Requires a GitHub personal access token with <code class="inline-code">repo</code> scope. See the API Keys step in Getting Started for how to add it to your config. Once connected, you can ask Claude to review a PR, explain a commit, or draft an issue description without switching apps.<br><br><strong>Classroom use:</strong> Ask Claude to review your latest PR diff and suggest improvements before you merge.<br><br><code class="code-block">npx -y @modelcontextprotocol/server-github</code><div class="card-takeaway"><strong>Key takeaway:</strong> If you only set up one server beyond Filesystem, make it GitHub. It fits right into the workflow you already have.</div></div>
          </div>
          <div class="glass-card reveal">
            <div class="card-num">03</div>
            <div class="card-title">Postgres: SQL queries against a live database</div>
            <div class="card-body-text">Runs read-only SQL queries against a PostgreSQL database and returns results as plain text. Claude can inspect your schema, write queries from plain-English descriptions, explain what existing queries do, and answer questions about your data without you writing a line of SQL. The connection string in your config acts as the credentials. Claude only gets the permissions that connection has, so a read-only user keeps things safe. Great for learning a new codebase's data model fast, or exploring a database you didn't build.<br><br><strong>Classroom use:</strong> Hand Claude an unfamiliar schema and ask it to explain each table in plain English.<br><br><code class="code-block">npx -y @modelcontextprotocol/server-postgres postgresql://localhost/mydb</code><div class="card-takeaway"><strong>Key takeaway:</strong> Point Claude at a database and ask questions in plain English. No SQL experience required.</div></div>
          </div>
          <div class="glass-card reveal">
            <div class="card-num">04</div>
            <div class="card-title">Brave Search: real-time web results</div>
            <div class="card-body-text">Queries the Brave Search API for live web results, bypassing Claude's training cutoff. Claude gets page titles, URLs, and descriptions, then synthesizes them into a structured answer. Requires a free Brave Search API key (see the API Keys step in Getting Started). Pair it with the Fetch server for a full research workflow: search returns the URLs, Fetch retrieves the full page text, and Claude synthesizes everything into one response.<br><br><strong>Classroom use:</strong> Research a topic and have Claude synthesize results from multiple sources in one response.<br><br><code class="code-block">npx -y @modelcontextprotocol/server-brave-search</code><div class="card-takeaway"><strong>Key takeaway:</strong> Use this when you need Claude to work with information that's newer than its training data.</div></div>
          </div>
          <div class="glass-card reveal">
            <div class="card-num">05</div>
            <div class="card-title">Puppeteer: browser automation and scraping</div>
            <div class="card-body-text">Controls a full headless Chrome browser, not just a plain HTTP fetcher. Claude can navigate to URLs, click buttons, fill forms, wait for JavaScript to finish rendering, take screenshots, and extract structured data from dynamic pages that a simple fetch can't handle. It runs real Chrome, so it works on React apps, infinite-scroll feeds, and any site that relies on JavaScript to render. No API key required.<br><br><strong>Classroom use:</strong> Scrape a product page for pricing data, or screenshot a dashboard for a weekly report.<br><br><code class="code-block">npx -y @modelcontextprotocol/server-puppeteer</code><div class="card-takeaway"><strong>Key takeaway:</strong> Reach for Puppeteer when a page uses JavaScript to render its content. A plain Fetch won't work on those.</div></div>
          </div>
          <div class="glass-card reveal">
            <div class="card-num">06</div>
            <div class="card-title">Slack: messages, channels, and users</div>
            <div class="card-body-text">Send and read messages, list channels, look up users, react to messages, and post formatted content to Slack workspaces, all from Claude. Requires a Slack bot token and your team ID set as environment variables (see the API Keys step in Getting Started). You'll need to create a Slack app at api.slack.com/apps, install it to your workspace, and grant it the right bot scopes. Once connected, Claude can draft responses, triage support channels, or post automated summaries.<br><br><strong>Classroom use:</strong> Have Claude monitor a support channel and draft responses to common questions.<br><br><code class="code-block">npx -y @modelcontextprotocol/server-slack</code><div class="card-takeaway"><strong>Key takeaway:</strong> Use this to bring Claude into team communication workflows: reading channels and drafting messages without copy-pasting.</div></div>
          </div>
          <div class="glass-card reveal">
            <div class="card-num">07</div>
            <div class="card-title">Fetch: retrieve content from any URL</div>
            <div class="card-body-text">Fetches raw HTML or plain text from any public URL. No browser, no JavaScript execution. It's the lightest way to pull in content from the web: docs pages, API responses, README files, RSS feeds. It doesn't run JavaScript, so it's fast and simple but only works on pages that render content server-side. No API key required. Pair it with Brave Search for a full research workflow: search returns the URLs, Fetch retrieves the full text.<br><br><strong>Classroom use:</strong> Point Claude at an API docs page and ask it to write a client based on what it reads.<br><br><code class="code-block">npx -y @modelcontextprotocol/server-fetch</code><div class="card-takeaway"><strong>Key takeaway:</strong> Use Fetch for static and server-rendered pages. Switch to Puppeteer if the page needs JavaScript to load its content.</div></div>
          </div>
        </div>
      </section>

      <hr class="section-divider" />

      <!-- ── Section 5: Use Cases ── -->
      <section class="section" id="use-cases">
        <div class="section-header reveal">
          <span class="section-eyebrow">05 / Applications</span>
          <h2 class="section-title">Use <span class="gradient-text">Cases</span></h2>
          <p class="section-desc">MCP servers get interesting when you combine them. These scenarios show how pairing the right servers with Claude solves real problems you'd otherwise grind through by hand.</p>
        </div>
        <div class="card-grid">
          <div class="glass-card reveal">
            <div class="card-num">01</div>
            <div class="card-title">Research Assistant</div>
            <div class="card-body-text"><strong>Problem:</strong> You need to research a topic across multiple sources and synthesize findings without copy-pasting from tab to tab.<br><br><strong>Servers:</strong> Brave Search + Fetch<br><br><strong>Outcome:</strong> Claude runs the search, fetches the full text of the top 3–5 results, cross-references them, and returns a structured summary with source citations, all in a single response. What normally takes 20 minutes of tab-switching takes under 30 seconds. The citations are real URLs you can verify yourself.<div class="card-takeaway"><strong>Key takeaway:</strong> The pattern: search → fetch → synthesize. One prompt replaces an entire manual research session.</div></div>
          </div>
          <div class="glass-card reveal">
            <div class="card-num">02</div>
            <div class="card-title">Code Reviewer</div>
            <div class="card-body-text"><strong>Problem:</strong> You want AI feedback on a pull request without pasting the whole diff into a chat window.<br><br><strong>Server:</strong> GitHub<br><br><strong>Outcome:</strong> Claude fetches the PR diff directly from GitHub, reads the changed files and their surrounding context, and returns structured review comments organized by file, pointing to specific lines. You get the same output as a human code review: what changed, what could break, what edge cases weren't handled. The feedback references real line numbers because it's reading the actual diff, not paraphrasing it.<div class="card-takeaway"><strong>Key takeaway:</strong> The pattern: fetch diff → analyze changes → structured output. Claude reviews your PR the same way a teammate would.</div></div>
          </div>
          <div class="glass-card reveal">
            <div class="card-num">03</div>
            <div class="card-title">Database Explorer</div>
            <div class="card-body-text"><strong>Problem:</strong> A teammate handed you a Postgres database with no documentation and you need to understand it fast.<br><br><strong>Server:</strong> Postgres<br><br><strong>Outcome:</strong> Claude queries the schema, describes each table in plain English, and maps out the relationships between them. It loaded the full schema in its first query, so you can ask follow-up questions like "which table tracks user sessions?" or "write a query that joins orders to users" and it already has the context to answer accurately. No copy-pasting schema dumps required.<div class="card-takeaway"><strong>Key takeaway:</strong> The pattern: read schema → answer questions in plain English. No documentation needed to understand an unfamiliar database.</div></div>
          </div>
          <div class="glass-card reveal">
            <div class="card-num">04</div>
            <div class="card-title">Web Scraper</div>
            <div class="card-body-text"><strong>Problem:</strong> You need structured data from a website that doesn't offer an API.<br><br><strong>Server:</strong> Puppeteer<br><br><strong>Outcome:</strong> Claude navigates to the page, waits for JavaScript to render, extracts the specific data you described (prices, names, dates, links), and returns it as a clean structured table. You can ask it to handle pagination, filter by criteria, or scrape multiple pages, all in plain English. The output is ready to paste into a spreadsheet or feed into another script.<div class="card-takeaway"><strong>Key takeaway:</strong> The pattern: navigate → wait → extract → structure. Describe the data you want in plain English; Claude handles the scraping logic.</div></div>
          </div>
          <div class="glass-card reveal">
            <div class="card-num">05</div>
            <div class="card-title">Team Automator</div>
            <div class="card-body-text"><strong>Problem:</strong> New GitHub issues pile up and it takes time to triage them, assign labels, and notify the team.<br><br><strong>Servers:</strong> GitHub + Slack<br><br><strong>Outcome:</strong> Claude reads every open issue, categorizes them by type (bug, feature request, question, docs), estimates priority based on the description and reactions, posts a formatted triage summary to a Slack channel, and suggests label assignments, all from a single prompt. What normally takes a 15-minute triage meeting becomes a one-liner. You review the suggestions, apply what looks right, and move on.<div class="card-takeaway"><strong>Key takeaway:</strong> The pattern: read → categorize → act across systems. Combining two servers turns a manual recurring workflow into a single prompt.</div></div>
          </div>
        </div>
      </section>

      <hr class="section-divider" />

      <!-- ── Section 6: Getting Started ── -->
      <section class="section" id="getting-started">
        <div class="section-header reveal">
          <span class="section-eyebrow">06 / Hands-on</span>
          <h2 class="section-title">Getting <span class="gradient-text">Started</span></h2>
          <p class="section-desc">Connect an MCP server to Claude CLI in four steps. We'll use the Filesystem server as the example. No API key needed, and it works immediately. If you prefer a GUI, Claude Desktop supports the same servers.</p>
        </div>
        <div class="steps-grid">
          <div class="step-card reveal">
            <div class="step-num">1</div>
            <div class="step-content">
              <div class="step-title">Install Claude CLI</div>
              <div class="step-desc">If you haven't already, install Claude CLI via npm. This is the host that manages your MCP connections.<code class="code-block">npm install -g @anthropic-ai/claude-code</code>Then log in:<code class="code-block">claude</code>Follow the prompts to authenticate with your Anthropic account.<br><br><strong>Prefer a GUI?</strong> Claude Desktop (anthropic.com/download) supports the same MCP servers. Configuration lives in a JSON file instead of the CLI.</div>
            </div>
          </div>
          <div class="step-card reveal">
            <div class="step-num">2</div>
            <div class="step-content">
              <div class="step-title">Add the Filesystem server</div>
              <div class="step-desc">Run this command to connect the Filesystem server. Replace <code class="inline-code">~/Documents</code> with the folder you want Claude to access.<code class="code-block">claude mcp add filesystem npx -y @modelcontextprotocol/server-filesystem ~/Documents</code>That's it. No config file to edit, no restart needed.</div>
            </div>
          </div>
          <div class="step-card reveal">
            <div class="step-num">3</div>
            <div class="step-content">
              <div class="step-title">Handle API keys</div>
              <div class="step-desc">Some servers need an API key. Pass them with the <code class="inline-code">-e</code> flag.<code class="code-block">claude mcp add brave-search -e BRAVE_API_KEY=your-key-here npx -y @modelcontextprotocol/server-brave-search</code><strong>Where to get keys for common servers</strong><br><strong>GitHub:</strong> github.com → Settings → Developer settings → Personal access tokens → Tokens (classic). Select the <code class="inline-code">repo</code> scope.<br><strong>Brave Search:</strong> brave.com/search/api. Free tier includes 2,000 queries/month.<br><strong>Slack:</strong> api.slack.com/apps. Create an app, install it to your workspace, and copy the Bot User OAuth Token.</div>
            </div>
          </div>
          <div class="step-card reveal">
            <div class="step-num">4</div>
            <div class="step-content">
              <div class="step-title">Verify it's working</div>
              <div class="step-desc">Check your connected servers:<code class="code-block">claude mcp list</code>You should see <code class="inline-code">filesystem</code> listed. Then start a chat and test it:<code class="code-block">claude</code>Type: <em>"List the files in my Documents folder."</em> Claude should call the <code class="inline-code">list_directory</code> tool and return the results. You'll see the tool call appear inline before the response.</div>
            </div>
          </div>
        </div>
        <div class="section-takeaway reveal"><strong>Key takeaway:</strong> Once you've done this once, adding new servers is just one <code class="inline-code">claude mcp add</code> command. The pattern never changes.</div>
      </section>

      <hr class="section-divider" />

      <!-- ── Section 7: Code Examples ── -->
      <section class="section" id="code-examples">
        <div class="section-header reveal">
          <span class="section-eyebrow">07 / Build</span>
          <h2 class="section-title">Code <span class="gradient-text">Examples</span></h2>
          <p class="section-desc">A minimal MCP server with one tool: <code class="inline-code">search_web(query)</code>. It calls the Brave Search API and returns the top five results. Pick your language. The structure is identical in both. Every MCP server follows three steps: <strong>(1)</strong> instantiate a server with a name, <strong>(2)</strong> register one or more tools with a name, description, and input schema, <strong>(3)</strong> connect a transport and run.</p>
        </div>
        <div class="tab-group reveal">
          <div class="tabs">
            <button type="button" class="tab-btn active" data-lang="python">Python</button>
            <button type="button" class="tab-btn" data-lang="typescript">TypeScript</button>
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
    response.raise_for_status()
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
    if (!res.ok) return { content: [{ type: "text", text: `Error: ${res.status} ${res.statusText}` }] };
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
        <div class="section-takeaway reveal"><strong>Key takeaway:</strong> Every MCP server follows the same three-part structure: instantiate, register tools, connect transport. Swap in different tool logic and it works for any use case.</div>
      </section>

    </div>
  </div>

  <!-- Presentation mode slide container -->
  <div class="slide-container" id="slide-container"></div>

  <!-- Presentation progress -->
  <div class="pres-progress" id="pres-progress"></div>
```

Note: The `<script>` tag follows immediately after this HTML. It will be replaced in the next tasks.

- [ ] **Step 2: Verify the HTML renders**

Open `index.html` in a browser. You should see the full-width layout with hero section, all 7 content sections with glass cards, and the floating top nav. Content should be readable. The particle background and animations won't work yet (those come in subsequent tasks).

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: restructure HTML with full-width layout, top nav, and hero"
```

---

### Task 3: Canvas Particle Background with Grid

**Files:**
- Modify: `index.html` (add particle system JS inside the `<script>` block)

This task implements the full-page animated canvas that renders:
1. Subtle cyan grid lines at ~40px intervals
2. 50 floating particles (mix of sky blue and purple)
3. Connection lines between nearby particles
4. Mouse interaction (particles attract toward cursor)

- [ ] **Step 1: Write the particle system**

Replace the entire `<script>` block with the new JavaScript. Start with the particle canvas system:

```javascript
// ── Particle Background ──
(function initParticles() {
  const canvas = document.getElementById('particle-canvas');
  if (!canvas) return;
  const ctx = canvas.getContext('2d');
  const dpr = window.devicePixelRatio || 1;
  let w, h;
  let mouseX = -9999, mouseY = -9999;
  const isMobile = window.innerWidth < 768;
  const PARTICLE_COUNT = isMobile ? 25 : 50;
  const CONNECT_DIST = 150;
  const MOUSE_DIST = 200;
  const GRID_SIZE = 40;

  function resize() {
    w = window.innerWidth;
    h = window.innerHeight;
    canvas.width = w * dpr;
    canvas.height = h * dpr;
    canvas.style.width = w + 'px';
    canvas.style.height = h + 'px';
    ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
  }
  window.addEventListener('resize', resize);
  resize();

  const particles = [];
  for (let i = 0; i < PARTICLE_COUNT; i++) {
    particles.push({
      x: Math.random() * w,
      y: Math.random() * h,
      vx: (Math.random() - 0.5) * 0.4,
      vy: (Math.random() - 0.5) * 0.4,
      r: 2 + Math.random() * 3,
      color: Math.random() > 0.5 ? '#38bdf8' : '#a855f7',
    });
  }

  document.addEventListener('mousemove', e => {
    mouseX = e.clientX;
    mouseY = e.clientY;
  });
  document.addEventListener('mouseleave', () => {
    mouseX = -9999;
    mouseY = -9999;
  });

  function drawGrid() {
    ctx.strokeStyle = 'rgba(6,182,212,0.03)';
    ctx.lineWidth = 1;
    ctx.beginPath();
    for (let x = 0; x <= w; x += GRID_SIZE) {
      ctx.moveTo(x, 0);
      ctx.lineTo(x, h);
    }
    for (let y = 0; y <= h; y += GRID_SIZE) {
      ctx.moveTo(0, y);
      ctx.lineTo(w, y);
    }
    ctx.stroke();
  }

  function frame() {
    ctx.clearRect(0, 0, w, h);
    drawGrid();

    for (const p of particles) {
      const dx = mouseX - p.x;
      const dy = mouseY - p.y;
      const dist = Math.sqrt(dx * dx + dy * dy);
      if (dist < MOUSE_DIST && dist > 0) {
        const force = (MOUSE_DIST - dist) / MOUSE_DIST * 0.02;
        p.vx += dx / dist * force;
        p.vy += dy / dist * force;
      }
      p.x += p.vx;
      p.y += p.vy;
      p.vx *= 0.99;
      p.vy *= 0.99;
      if (p.x < 0) { p.x = 0; p.vx *= -1; }
      if (p.x > w) { p.x = w; p.vx *= -1; }
      if (p.y < 0) { p.y = 0; p.vy *= -1; }
      if (p.y > h) { p.y = h; p.vy *= -1; }
    }

    for (let i = 0; i < particles.length; i++) {
      for (let j = i + 1; j < particles.length; j++) {
        const a = particles[i], b = particles[j];
        const dx = a.x - b.x, dy = a.y - b.y;
        const dist = Math.sqrt(dx * dx + dy * dy);
        if (dist < CONNECT_DIST) {
          const opacity = 1 - dist / CONNECT_DIST;
          const mouseDist = Math.min(
            Math.sqrt((mouseX - (a.x + b.x) / 2) ** 2 + (mouseY - (a.y + b.y) / 2) ** 2),
            MOUSE_DIST
          );
          const brightness = 0.12 + (1 - mouseDist / MOUSE_DIST) * 0.08;
          ctx.strokeStyle = a.color === '#38bdf8'
            ? `rgba(56,189,248,${opacity * brightness})`
            : `rgba(168,85,247,${opacity * brightness})`;
          ctx.lineWidth = 1;
          ctx.beginPath();
          ctx.moveTo(a.x, a.y);
          ctx.lineTo(b.x, b.y);
          ctx.stroke();
        }
      }
    }

    for (const p of particles) {
      ctx.beginPath();
      ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
      ctx.fillStyle = p.color;
      ctx.fill();
      ctx.shadowColor = p.color;
      ctx.shadowBlur = 8;
      ctx.fill();
      ctx.shadowBlur = 0;
    }

    requestAnimationFrame(frame);
  }
  frame();
})();
```

- [ ] **Step 2: Verify the particle background**

Open `index.html` in a browser. You should see:
- Faint cyan grid lines across the entire page
- ~50 floating dots (blue and purple) drifting slowly
- Lines connecting nearby particles
- Moving your mouse should gently attract nearby particles

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add canvas particle background with grid and mouse interaction"
```

---

### Task 4: Hero Orbital Animation

**Files:**
- Modify: `index.html` (add orbital animation JS to the `<script>` block)

- [ ] **Step 1: Add the orbital animation system**

Append this after the particle system in the `<script>` block:

```javascript
// ── Hero Orbital Animation ──
(function initOrbital() {
  const container = document.getElementById('orbital');
  if (!container) return;

  const nodes = [
    { ring: 1, radius: 80, speed: 0.008, offset: 0, color: '#38bdf8', label: 'TOOLS' },
    { ring: 1, radius: 80, speed: 0.008, offset: Math.PI, color: '#a855f7', label: 'RESOURCES' },
    { ring: 2, radius: 130, speed: -0.005, offset: 0.5, color: '#38bdf8', label: 'PROMPTS' },
    { ring: 2, radius: 130, speed: -0.005, offset: 2.6, color: '#a855f7', label: 'APIS' },
    { ring: 3, radius: 180, speed: 0.003, offset: 1, color: '#38bdf8', label: 'DATA' },
    { ring: 3, radius: 180, speed: 0.003, offset: 3.5, color: '#a855f7', label: 'MODELS' },
  ];

  const nodeEls = [];
  const labelEls = [];
  const center = { x: container.offsetWidth / 2, y: container.offsetHeight / 2 };

  nodes.forEach(n => {
    const el = document.createElement('div');
    el.className = 'orbit-node';
    el.style.width = '10px';
    el.style.height = '10px';
    el.style.background = n.color;
    el.style.boxShadow = `0 0 12px ${n.color}`;
    container.appendChild(el);
    nodeEls.push(el);

    const lbl = document.createElement('div');
    lbl.className = 'orbit-node-label';
    lbl.textContent = n.label;
    container.appendChild(lbl);
    labelEls.push(lbl);
  });

  const pulses = [];
  for (let i = 0; i < 3; i++) {
    const el = document.createElement('div');
    el.className = 'orbit-pulse';
    container.appendChild(el);
    pulses.push({ el, fromIdx: 0, toIdx: 1, progress: Math.random(), speed: 0.005 + Math.random() * 0.005 });
  }

  let t = 0;
  function animate() {
    t++;
    const cx = container.offsetWidth / 2;
    const cy = container.offsetHeight / 2;

    nodes.forEach((n, i) => {
      const angle = n.offset + t * n.speed;
      const x = cx + Math.cos(angle) * n.radius;
      const y = cy + Math.sin(angle) * n.radius;
      nodeEls[i].style.left = (x - 5) + 'px';
      nodeEls[i].style.top = (y - 5) + 'px';
      labelEls[i].style.left = (x + 14) + 'px';
      labelEls[i].style.top = (y - 6) + 'px';
    });

    pulses.forEach(p => {
      p.progress += p.speed;
      if (p.progress >= 1) {
        p.progress = 0;
        p.fromIdx = Math.floor(Math.random() * nodes.length);
        p.toIdx = (p.fromIdx + 1 + Math.floor(Math.random() * (nodes.length - 1))) % nodes.length;
      }
      const fromEl = nodeEls[p.fromIdx];
      const toEl = nodeEls[p.toIdx];
      const fx = parseFloat(fromEl.style.left) + 5;
      const fy = parseFloat(fromEl.style.top) + 5;
      const tx = parseFloat(toEl.style.left) + 5;
      const ty = parseFloat(toEl.style.top) + 5;
      const px = fx + (tx - fx) * p.progress;
      const py = fy + (ty - fy) * p.progress;
      p.el.style.left = (px - 2) + 'px';
      p.el.style.top = (py - 2) + 'px';
      p.el.style.opacity = Math.sin(p.progress * Math.PI) * 0.8;
    });

    requestAnimationFrame(animate);
  }
  animate();
})();
```

- [ ] **Step 2: Verify the orbital animation**

Open `index.html` in a browser. In the hero section, the right side should show:
- 3 concentric orbit rings
- 6 labeled nodes orbiting at different speeds and directions
- 3 light pulses traveling between random node pairs

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add hero orbital animation with nodes and data pulses"
```

---

### Task 5: Hero Load Animations and Scroll Reveal System

**Files:**
- Modify: `index.html` (add animation JS to the `<script>` block)

- [ ] **Step 1: Add the hero load animation trigger**

Append this after the orbital animation in the `<script>` block:

```javascript
// ── Hero Load Animation ──
(function initHeroAnim() {
  window.addEventListener('load', () => {
    document.querySelectorAll('.hero-anim').forEach(el => {
      const delay = parseFloat(el.style.transitionDelay) || 0;
      setTimeout(() => el.classList.add('loaded'), delay * 1000);
    });
    document.querySelectorAll('.hero-graphic-anim').forEach(el => {
      const delay = parseFloat(el.style.transitionDelay) || 0;
      setTimeout(() => el.classList.add('loaded'), delay * 1000);
    });
  });
})();
```

- [ ] **Step 2: Add the scroll reveal system**

Append this after the hero animation:

```javascript
// ── Scroll Reveal ──
(function initReveal() {
  const reveals = document.querySelectorAll('.reveal');
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        const el = entry.target;
        const section = el.closest('.section') || el.closest('.hero');
        const siblings = section
          ? [...section.querySelectorAll('.reveal')]
          : [el];
        const idx = siblings.indexOf(el);
        const delay = idx * 0.1;
        el.style.transitionDelay = delay + 's';
        el.classList.add('visible');
        observer.unobserve(el);
      }
    });
  }, { threshold: 0.15 });
  reveals.forEach(el => observer.observe(el));
})();
```

- [ ] **Step 3: Verify animations**

Open `index.html` in a browser:
- On load, the hero eyebrow, title lines, description, and CTAs should fade/slide in sequentially
- The orbital graphic should scale up from the center
- Scrolling down, each section title and card should fade up as it enters the viewport, staggered within each section

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add hero load animations and scroll-triggered content reveals"
```

---

### Task 6: Scroll-Spy, Tab Switcher, and Hamburger Menu

**Files:**
- Modify: `index.html` (add navigation JS to the `<script>` block)

- [ ] **Step 1: Add scroll-spy for the top nav**

Append this after the scroll reveal system:

```javascript
// ── Scroll Spy ──
(function initScrollSpy() {
  const navLinks = document.querySelectorAll('.nav-links a[data-section]');
  const spy = new IntersectionObserver(entries => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        navLinks.forEach(link => link.classList.remove('active'));
        const hit = document.querySelector(`.nav-links a[data-section="${entry.target.id}"]`);
        if (hit) hit.classList.add('active');
      }
    });
  }, { threshold: 0, rootMargin: '-40% 0px -55% 0px' });
  document.querySelectorAll('section[id]').forEach(s => spy.observe(s));
})();
```

- [ ] **Step 2: Add the tab switcher**

```javascript
// ── Tab Switcher ──
document.querySelectorAll('.tab-btn').forEach(btn => {
  btn.addEventListener('click', () => {
    const group = btn.closest('.tab-group');
    group.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
    group.querySelectorAll('.tab-panel').forEach(p => p.classList.remove('active'));
    btn.classList.add('active');
    group.querySelector(`.tab-panel[data-lang="${btn.dataset.lang}"]`).classList.add('active');
  });
});
```

- [ ] **Step 3: Add hamburger menu toggle**

```javascript
// ── Hamburger Menu ──
(function initHamburger() {
  const btn = document.getElementById('hamburger');
  const nav = document.getElementById('mobile-nav');
  if (!btn || !nav) return;
  btn.addEventListener('click', () => nav.classList.toggle('open'));
  nav.querySelectorAll('a').forEach(a => {
    a.addEventListener('click', () => nav.classList.remove('open'));
  });
})();
```

- [ ] **Step 4: Verify navigation**

Open `index.html` in a browser:
- Scrolling should highlight the matching nav link in cyan
- The Python/TypeScript tabs in the Code Examples section should switch panels
- At tablet width (below 1024px), the hamburger should appear and toggle the mobile nav

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add scroll-spy, tab switcher, and responsive hamburger menu"
```

---

### Task 7: Presentation Mode — Core Engine

**Files:**
- Modify: `index.html` (add presentation mode JS to the `<script>` block)

This is the most complex task. It implements:
- Cloning each section into a full-screen slide
- Enter/exit presentation mode (button, keyboard, hero CTA)
- Arrow key navigation between slides
- Default cinematic fade+scale transitions
- Progress dots at bottom
- Cursor auto-hide after 3s

- [ ] **Step 1: Add the presentation mode engine**

Append this after the hamburger menu JS:

```javascript
// ── Presentation Mode ──
(function initPresentation() {
  const container = document.getElementById('slide-container');
  const progress = document.getElementById('pres-progress');
  const presentBtn = document.getElementById('present-btn');
  const heroPresentBtn = document.getElementById('hero-present-btn');
  const topNav = document.querySelector('.top-nav');
  if (!container || !progress) return;

  const sectionIds = ['hero', 'what-is-mcp', 'how-it-works', 'core-capabilities', 'popular-servers', 'use-cases', 'getting-started', 'code-examples'];
  const DISSOLVE_TRANSITIONS = [[0, 1], [3, 4]];
  let slides = [];
  let currentSlide = 0;
  let isPresenting = false;
  let transitioning = false;
  let cursorTimer = null;

  function buildSlides() {
    container.innerHTML = '';
    progress.innerHTML = '';
    slides = [];

    sectionIds.forEach((id, i) => {
      const source = document.getElementById(id);
      if (!source) return;

      const slide = document.createElement('div');
      slide.className = 'slide';
      slide.dataset.index = i;

      const content = document.createElement('div');
      content.className = 'content-wrap';
      content.style.maxWidth = '1200px';
      content.style.width = '100%';
      content.innerHTML = source.innerHTML;

      content.querySelectorAll('.reveal').forEach(el => {
        el.classList.add('visible');
        el.style.transitionDelay = '0s';
      });

      slide.appendChild(content);
      container.appendChild(slide);
      slides.push(slide);

      const dot = document.createElement('div');
      dot.className = 'pres-dot';
      dot.addEventListener('click', () => goToSlide(i));
      progress.appendChild(dot);
    });

    const counter = document.createElement('span');
    counter.className = 'pres-counter';
    counter.id = 'pres-counter';
    progress.appendChild(counter);
  }

  function updateProgress() {
    progress.querySelectorAll('.pres-dot').forEach((dot, i) => {
      dot.classList.toggle('active', i === currentSlide);
    });
    const counter = document.getElementById('pres-counter');
    if (counter) counter.textContent = `${currentSlide + 1} / ${slides.length}`;
  }

  function isDissolveTransition(from, to) {
    return DISSOLVE_TRANSITIONS.some(([a, b]) => a === from && b === to);
  }

  function staggerRevealIn(slide) {
    const els = slide.querySelectorAll('.glass-card, .step-card, .section-header, .hero-text, .hero-graphic, .tab-group, .section-takeaway');
    els.forEach((el, i) => {
      el.style.opacity = '0';
      el.style.transform = 'translateY(30px)';
      el.style.transition = 'none';
      requestAnimationFrame(() => {
        requestAnimationFrame(() => {
          el.style.transition = `opacity 0.5s ease-out ${i * 0.1}s, transform 0.5s ease-out ${i * 0.1}s`;
          el.style.opacity = '1';
          el.style.transform = 'translateY(0)';
        });
      });
    });
  }

  function dissolveOut(slide) {
    const els = slide.querySelectorAll('.glass-card, .step-card, .section-header, .hero-text, .hero-graphic, .tab-group, .section-takeaway');
    els.forEach((el, i) => {
      const dx = (Math.random() - 0.5) * 400;
      const dy = (Math.random() - 0.5) * 400;
      const dr = (Math.random() - 0.5) * 30;
      el.style.transition = `opacity 0.5s ease-out ${i * 0.05}s, transform 0.5s ease-out ${i * 0.05}s`;
      el.style.opacity = '0';
      el.style.transform = `translate(${dx}px, ${dy}px) rotate(${dr}deg)`;
    });
  }

  function dissolveIn(slide) {
    const els = slide.querySelectorAll('.glass-card, .step-card, .section-header, .hero-text, .hero-graphic, .tab-group, .section-takeaway');
    els.forEach((el, i) => {
      const dx = (Math.random() - 0.5) * 400;
      const dy = (Math.random() - 0.5) * 400;
      const dr = (Math.random() - 0.5) * 30;
      el.style.opacity = '0';
      el.style.transform = `translate(${dx}px, ${dy}px) rotate(${dr}deg)`;
      el.style.transition = 'none';
      requestAnimationFrame(() => {
        requestAnimationFrame(() => {
          el.style.transition = `opacity 0.6s ease-out ${i * 0.08}s, transform 0.6s ease-out ${i * 0.08}s`;
          el.style.opacity = '1';
          el.style.transform = 'translate(0, 0) rotate(0deg)';
        });
      });
    });
  }

  function goToSlide(idx) {
    if (transitioning || idx === currentSlide || idx < 0 || idx >= slides.length) return;
    transitioning = true;

    const from = currentSlide;
    const to = idx;
    const useDissolve = isDissolveTransition(from, to);
    const oldSlide = slides[from];
    const newSlide = slides[to];

    if (useDissolve) {
      dissolveOut(oldSlide);
      setTimeout(() => {
        oldSlide.classList.remove('active');
        oldSlide.style.opacity = '0';
        newSlide.classList.add('active');
        newSlide.style.opacity = '1';
        newSlide.style.transform = 'scale(1)';
        dissolveIn(newSlide);
        setTimeout(() => { transitioning = false; }, 700);
      }, 550);
    } else {
      oldSlide.classList.add('exiting');
      oldSlide.classList.remove('active');
      setTimeout(() => {
        oldSlide.classList.remove('exiting');
        oldSlide.style.opacity = '0';
        newSlide.classList.add('active');
        staggerRevealIn(newSlide);
        setTimeout(() => { transitioning = false; }, 600);
      }, 400);
    }

    currentSlide = to;
    updateProgress();
  }

  function enterPresentation() {
    if (isPresenting) return;
    isPresenting = true;
    buildSlides();
    document.body.classList.add('presenting');
    topNav.classList.add('hidden');
    currentSlide = 0;
    slides[0].classList.add('active');
    slides[0].style.opacity = '1';
    slides[0].style.transform = 'scale(1)';
    staggerRevealIn(slides[0]);
    updateProgress();
    startCursorHide();
  }

  function exitPresentation() {
    if (!isPresenting) return;
    isPresenting = false;
    document.body.classList.remove('presenting');
    document.body.classList.remove('cursor-hidden');
    topNav.classList.remove('hidden');
    container.innerHTML = '';
    progress.innerHTML = '';
    stopCursorHide();
  }

  function startCursorHide() {
    document.addEventListener('mousemove', resetCursorTimer);
    resetCursorTimer();
  }

  function stopCursorHide() {
    document.removeEventListener('mousemove', resetCursorTimer);
    clearTimeout(cursorTimer);
  }

  function resetCursorTimer() {
    document.body.classList.remove('cursor-hidden');
    clearTimeout(cursorTimer);
    cursorTimer = setTimeout(() => {
      if (isPresenting) document.body.classList.add('cursor-hidden');
    }, 3000);
  }

  document.addEventListener('keydown', e => {
    if (!isPresenting) {
      if (e.key === 'p' || e.key === 'P' || e.key === 'F5') {
        e.preventDefault();
        enterPresentation();
      }
      return;
    }
    if (e.key === 'Escape') {
      exitPresentation();
    } else if (e.key === 'ArrowRight' || e.key === 'ArrowDown') {
      e.preventDefault();
      goToSlide(currentSlide + 1);
    } else if (e.key === 'ArrowLeft' || e.key === 'ArrowUp') {
      e.preventDefault();
      goToSlide(currentSlide - 1);
    }
  });

  if (presentBtn) presentBtn.addEventListener('click', enterPresentation);
  if (heroPresentBtn) heroPresentBtn.addEventListener('click', enterPresentation);
})();
```

- [ ] **Step 2: Verify presentation mode**

Open `index.html` in a browser and test:
1. Click "Present" button in top nav or "Present ▶" in hero — should enter full-screen slide mode
2. Arrow keys should navigate between slides
3. The first transition (slide 0→1) should use particle dissolve effect
4. Other transitions should use cinematic fade+scale with staggered content reveal
5. Progress dots at bottom should update and be clickable
6. Mouse cursor should auto-hide after 3 seconds of inactivity
7. Press Escape to exit — should return to normal scrollable page
8. Press P to re-enter presentation mode

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add presentation mode with cinematic and particle dissolve transitions"
```

---

### Task 8: Final Polish and Integration Testing

**Files:**
- Modify: `index.html` (minor fixes and integration)

- [ ] **Step 1: Close the HTML document**

Make sure the `<script>` block is properly closed and the document ends with:

```html
</script>
</body>
</html>
```

- [ ] **Step 2: Full integration test in browser**

Open `index.html` and test the complete experience:

1. **Load:** Hero animates in sequentially (eyebrow → title → description → CTAs → orbital graphic)
2. **Background:** Particle canvas with grid, particles drift, react to mouse
3. **Scroll:** Content fades up section by section, staggered within each section
4. **Nav:** Top nav scroll-spy highlights active section, hamburger works on mobile
5. **Tabs:** Python/TypeScript code switcher works
6. **Present mode:** Enter via button/keyboard, navigate with arrows, dissolve on slides 0→1 and 3→4, fade+scale on others, progress dots work, Escape exits
7. **Responsive:** Check at 1024px (tablet), 768px (mobile) — layout collapses cleanly
8. **Content:** Verify all 7 sections' text content matches the original site exactly
9. **Console:** No JS errors in the browser console

- [ ] **Step 3: Commit final state**

```bash
git add index.html
git commit -m "feat: complete investor UI redesign with animations and presentation mode"
```

---

## Summary

| Task | Description | Estimated Work |
|------|-------------|----------------|
| 1 | CSS foundation (variables, layout, animations, responsive) | Large — full stylesheet |
| 2 | HTML restructure (nav, hero, all 7 sections as glass cards) | Large — full body rewrite |
| 3 | Canvas particle background with grid | Medium — ~80 lines JS |
| 4 | Hero orbital animation | Medium — ~80 lines JS |
| 5 | Hero load animations + scroll reveal system | Small — ~30 lines JS |
| 6 | Scroll-spy, tabs, hamburger | Small — ~30 lines JS |
| 7 | Presentation mode engine | Large — ~150 lines JS |
| 8 | Final polish and integration test | Small — testing only |
