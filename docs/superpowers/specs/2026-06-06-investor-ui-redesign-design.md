# MCP Guide Site — Investor Presentation Redesign

**Date:** 2026-06-06
**Status:** Draft
**Goal:** Complete UI overhaul of the MCP guide site to create a flashy, investor-ready presentation experience while retaining all existing informational content.

---

## 1. Visual Identity

### Background
- **Base:** Deep navy gradient (`#020617` → `#0f172a`)
- **Grid overlay:** Subtle cyan grid lines (`rgba(6,182,212,0.03)`) repeating at ~40px intervals, both horizontal and vertical
- **Particles:** Animated floating nodes in sky blue (`#38bdf8`) and purple (`#a855f7`), connected by faint lines that shift opacity. Nodes drift slowly, connections form and dissolve. Rendered on a full-page `<canvas>` behind all content
- **Glow accents:** Soft radial gradients behind key content areas to create depth

### Color Palette
| Role | Value | Usage |
|------|-------|-------|
| Background | `#020617` → `#0f172a` | Page base gradient |
| Grid lines | `rgba(6,182,212,0.03)` | Structural backdrop |
| Primary accent | `#38bdf8` (sky blue) | Headings, active states, links |
| Secondary accent | `#a855f7` (purple) | Gradient endpoints, highlights |
| Text primary | `#f1f5f9` | Headings, body text |
| Text secondary | `rgba(255,255,255,0.5)` | Descriptions, muted text |
| Text dim | `rgba(255,255,255,0.35)` | Captions, meta text |
| Card/surface | `rgba(15,23,42,0.6)` | Card backgrounds with backdrop-blur |
| Border | `rgba(56,189,248,0.1)` | Card edges, dividers |

### Typography
- **UI font:** Inter (weights: 400, 500, 600, 700, 800)
- **Code font:** Fira Code (weights: 400, 500)
- **Hero title:** ~48–64px, weight 800, tight letter-spacing (-1.5px)
- **Section titles:** ~36–48px, weight 800
- **Body:** 15–16px, line-height 1.6–1.7
- **Labels/eyebrows:** 11–13px, uppercase, letter-spacing 2–4px

---

## 2. Layout Structure

### Normal Browse Mode
- **No sidebar.** Full-width content, centered with max-width (~1200px)
- **Floating top nav bar:** Fixed to top, semi-transparent with backdrop-blur. Contains:
  - Left: Brand mark/logo ("Dixie Tech · AI Division")
  - Center/Right: Section links (horizontal, text-only, subtle)
  - Far right: Presentation mode icon button (play/present icon)
- Nav uses scroll-spy to highlight the active section
- Sections flow vertically, each taking generous vertical space (not necessarily full-viewport in browse mode, but visually separated)

### Responsive Behavior
- **Desktop (>1024px):** Full layout as described
- **Tablet (768–1024px):** Nav collapses to hamburger menu, content scales down
- **Mobile (<768px):** Stacked layout, presentation mode disabled or simplified, hamburger nav

---

## 3. Hero Section

Split layout, full viewport height:

- **Left side (text):**
  - Eyebrow: "DIXIE TECH · AI DIVISION" in small uppercase, cyan-tinted
  - Main title in two lines: "We Taught AI" (white) / "to Use Tools." (gradient: sky blue → purple)
  - Short description below: "An open protocol connecting AI models to every API, database, and service your business runs on." in dim text
  - Optional CTA buttons: "Explore" (outline) and "Present ▶" (gradient fill)

- **Right side (animated graphic):**
  - Orbital/connection visualization: concentric circles with nodes orbiting at different speeds
  - Nodes represent MCP concepts (tools, resources, data) — connected by animated lines
  - Pulses of light travel along connections to suggest data flow
  - Pure CSS/JS animation, no external libraries required

- **Animation on load:**
  - Eyebrow fades in first (0.3s delay)
  - Title lines slide up and fade in sequentially (0.5s, 0.8s)
  - Description fades in (1.0s)
  - CTAs fade in (1.2s)
  - Orbital graphic scales up from center and begins animating (0.5s)

---

## 4. Content Sections

All 7 existing sections are preserved with their current informational content. No content is added or removed — only the presentation layer changes.

### Sections (in order)
1. **What is MCP?** — 3 content blocks
2. **How it Works** — 3 content blocks
3. **Core Capabilities** — 3 content blocks (Tools, Resources, Prompts)
4. **Popular Servers** — 7 server cards (Filesystem, GitHub, Postgres, Brave Search, Puppeteer, Slack, Fetch)
5. **Use Cases** — 5 use case blocks
6. **Getting Started** — 4-6 step-by-step blocks with CLI commands
7. **Code Examples** — Tabbed code viewer (Python/TypeScript)

### Content Display (Browse Mode)
- **No accordions.** All content within a section is visible — no click-to-expand.
- Content blocks are styled as cards or distinct visual units with the glass-morphism surface treatment (semi-transparent background, backdrop-blur, subtle border)
- Cards arranged in responsive grids (2–3 columns on desktop, 1 on mobile)

### Scroll Animations (Browse Mode)
Every content element uses IntersectionObserver to trigger entrance animations:
- **Fade up:** Element starts 30–40px below final position, opacity 0 → 1, translateY → 0. Duration ~0.6s, ease-out
- **Stagger:** Within a section, each card/block delays by 0.1–0.15s after the previous one
- **Once only:** Animation triggers once when element enters viewport (threshold ~0.15), does not replay on re-scroll

### Section Titles
- Large, bold text with gradient or solid color
- Subtle accent line or glow beneath
- Small eyebrow label above ("01 / CONCEPTS", "02 / ARCHITECTURE", etc.) for visual rhythm

### Code Blocks
- Dark surface with slightly different background than cards
- Syntax highlighting in the blue/purple palette
- Tab switcher for Python/TypeScript — styled as pill buttons with active state

---

## 5. Presentation Mode

### Entering/Exiting
- **Button:** Small icon in the top nav (play triangle or expand icon). Tooltip: "Present (P)"
- **Keyboard:** Press `P` or `F5` to enter. Press `Esc` to exit.
- **Hero CTA:** "Present ▶" button on the hero also enters presentation mode.

### Slide Mapping
Each of the 7 content sections becomes one slide. The hero is slide 0. Total: 8 slides.

### Slide Layout
- Content is centered on screen, vertically and horizontally
- Section title appears prominently
- Content blocks arrange in the same grid layout but scaled/spaced for full-viewport display
- No scrolling within a slide — content must fit the viewport (cards may be slightly smaller, or overflow is handled with a subtle scroll indicator if absolutely necessary)

### Navigation
- **Arrow keys:** Left/Up = previous slide, Right/Down = next slide
- **Click zones:** (optional) Left 20% of screen = back, right 20% = forward
- **Progress indicator:** Small dots or a thin progress bar at the bottom of the screen showing current position (e.g., 3/8)
- **Top nav hides** when presentation mode is active (slides up and out)

### Transitions — Default: Cinematic Fade + Scale
- Current slide: opacity 1 → 0, scale 1 → 0.95, duration 0.4s
- Next slide: opacity 0 → 1, scale 1.05 → 1, duration 0.5s, delay 0.15s
- Content elements within the new slide stagger in: each card/block fades up with 0.1s incremental delay

### Transitions — Special: Particle Dissolve
Used on specific transitions for maximum impact:
- **Slide 0 → 1** (Hero → What is MCP): first transition sets the tone
- **Slide 3 → 4** (Core Capabilities → Popular Servers): marks the shift from concepts to concrete tools

Effect:
- Current slide's content elements break apart — each element gets a random translateX/Y (±200px) and opacity → 0, with slight rotation. Duration 0.5s, staggered by 0.05s per element
- Brief pause (0.2s) where only the particle background is visible
- Next slide's elements coalesce from scattered positions to their final layout, opacity 0 → 1. Duration 0.6s, staggered

### Presentation Mode UI
- Slide counter: bottom-center, small text "3 / 8" or dot indicators
- Subtle gradient vignette at screen edges for focus
- Mouse cursor auto-hides after 3s of inactivity, reappears on movement

---

## 6. Animated Background System

### Canvas Particle Network
A full-page `<canvas>` element sits behind all content (z-index: 0). It renders:

1. **Grid lines:** Faint cyan lines at ~40px intervals, slight opacity variation
2. **Particles:** 40–60 floating dots, mix of sky blue and purple, sizes 2–5px
3. **Connections:** Lines drawn between particles within a proximity threshold (~150px). Opacity inversely proportional to distance. Color interpolated between the two connected particle colors
4. **Movement:** Particles drift in random directions at very slow speed (0.2–0.5px per frame). Bounce off viewport edges or wrap around
5. **Mouse interaction:** Particles within ~200px of cursor gently attract toward it, creating a subtle responsive effect. Connections near cursor brighten slightly

### Performance
- Use `requestAnimationFrame` for smooth 60fps
- Throttle particle count on mobile (20–30 particles)
- `will-change: transform` on any overlapping elements
- Canvas renders at device pixel ratio for crispness

---

## 7. Animations Inventory

| Element | Trigger | Animation | Duration | Easing |
|---------|---------|-----------|----------|--------|
| Hero eyebrow | Page load | Fade in | 0.5s, delay 0.3s | ease-out |
| Hero title line 1 | Page load | Fade up | 0.6s, delay 0.5s | ease-out |
| Hero title line 2 | Page load | Fade up | 0.6s, delay 0.8s | ease-out |
| Hero description | Page load | Fade in | 0.5s, delay 1.0s | ease-out |
| Hero CTAs | Page load | Fade in | 0.5s, delay 1.2s | ease-out |
| Hero orbital graphic | Page load | Scale up + rotate start | 0.8s, delay 0.5s | ease-out |
| Orbital nodes | Continuous | Orbit at varying speeds | Infinite | linear |
| Orbital pulses | Continuous | Travel along connections | 2–3s loop | linear |
| Section title | Scroll into view | Fade up | 0.6s | ease-out |
| Content card | Scroll into view | Fade up, staggered | 0.6s, +0.1s each | ease-out |
| Nav link active | Scroll spy | Underline slide in | 0.3s | ease |
| Slide transition (default) | Presentation nav | Fade + scale | 0.4s out, 0.5s in | ease-in-out |
| Slide transition (dissolve) | Presentation nav (special) | Scatter + coalesce | 0.5s + 0.6s | ease-out |
| Background particles | Continuous | Drift + connect | Infinite | linear |
| Cursor near particles | Mouse move | Attract toward cursor | Continuous | — |

---

## 8. Technical Approach

- **Single file:** Remains a single `index.html` with inline `<style>` and `<script>` — no build step, no dependencies
- **External fonts only:** Google Fonts (Inter, Fira Code) — same as current
- **Canvas API:** For particle background — vanilla JS, no libraries
- **CSS animations + JS IntersectionObserver:** For scroll-triggered reveals
- **CSS transitions:** For presentation mode slide changes
- **No frameworks.** Pure HTML/CSS/JS

---

## 9. Content Preservation

All existing textual content is preserved exactly:
- Section titles and descriptions
- Card content (all 7 sections worth)
- Code examples (Python and TypeScript)
- CLI commands and setup steps
- Links (modelcontextprotocol GitHub, etc.)
- Branding ("Dixie Tech · AI Division")

The only changes are to the presentation layer — layout, styling, animations, and interaction patterns. No informational content is added, removed, or rewritten.

---

## 10. Out of Scope

- No backend or database
- No user accounts or authentication
- No CMS or content editing interface
- No external JavaScript libraries or frameworks
- No build tooling (webpack, vite, etc.)
- No mobile-specific presentation mode (presentation mode targets desktop/laptop screens for live investor demos)
