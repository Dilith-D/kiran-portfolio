# Portfolio Context — Kiran Krishnakumar

> This file is the single source of truth for how this portfolio was built. If you are continuing development with any AI model (Claude, GPT, Gemini, etc.) or Claude Code, paste this file first. It covers every design decision, technical detail, and content structure.

---

## Live URL

**https://kirankrishnakumar.vercel.app**

---

## Project Structure

```
kiran portfolio/
├── index.html                    ← the entire site (HTML + CSS + JS, single file)
├── favicon.svg                   ← SVG favicon (bold KK, cyan on dark, used by modern browsers)
├── favicon.ico                   ← ICO favicon with 16×16, 32×32, 48×48 sizes (fallback)
├── assets/
│   └── img/
│       ├── home-perfil.png       ← Kiran's profile photo
│       └── favicon-32.png        ← 32×32 PNG favicon (extra fallback)
├── vercel.json                   ← Vercel SPA rewrite rule
├── _redirects                    ← Netlify equivalent (not used, kept for reference)
├── sitemap.xml                   ← all 6 section URLs for search engines
├── llms.txt                      ← AI-readable structured profile (llmstxt.org spec)
├── context.md                    ← this file
└── kiran_portfolio_build_plan.md ← original brief
```

**Architecture decision:** Everything lives in `index.html` — all CSS in `<style>`, all JS in `<script>`. No build step, no npm, no framework. Deployable by drag-and-drop or one CLI command. This was a deliberate choice for simplicity and portability.

---

## Deployment

**Platform:** Vercel  
**Deploy command** (from project folder):
```bash
vercel --prod --yes
```
**SPA routing:** `vercel.json` rewrites all paths to `index.html` so clean URLs like `/certifications` work on direct access or refresh.

---

## Design System

### Fonts (Google CDN)
| Font | Usage |
|---|---|
| **Syne** (700) | All headings (h1, h2, h3), stat numbers |
| **JetBrains Mono** (400/500) | Section labels, tags, code, monospace elements |
| **DM Sans** (400/500) | Body text, nav links, descriptions |

### Colour Palette
```css
--bg-base:     #050810   /* page background */
--bg-surface:  #0c1220   /* alternate sections */
--bg-elevated: #111827   /* cards */

--accent-primary: #00c8ff   /* cyan  — structure, borders, interactive UI */
--accent-warm:    #f59e0b   /* amber — data values, dates, codes, stats */

--text-primary:   #f0f4ff
--text-secondary: #8892a4
--text-muted:     #4a5568

--border:       rgba(0, 200, 255, 0.12)
--border-hover: rgba(0, 200, 255, 0.40)
--radius:    12px
--radius-sm:  6px
--nav-height: 70px
```

**Colour logic (important):** Cyan is used only for structural/interface elements (borders, nav underlines, pipeline connectors). Amber is used only for data values (stats, dates, certification codes). This cold/warm split prevents the site from feeling uniformly "vibe-coded blue" and creates visual hierarchy.

### Section Backgrounds (alternating)
```
Hero         → gradient (#050810 → #0c1a2e → #050810)
About        → --bg-base
Metrics      → --bg-base
Experience   → --bg-surface
Skills       → --bg-base
DAG          → --bg-base
Certifications → --bg-surface
Contact      → --bg-base
Footer       → --bg-surface
```

---

## Page Sections (in order)

### 1. Nav
- Fixed, 70px tall, `backdrop-filter: blur(12px)`
- Transparent until scrolled 80px, then `rgba(5,8,16,0.92)` with bottom border
- Logo: `KK.` (Syne 700, cyan dot)
- Links: About / Experience / Skills / Certifications / Contact
- Right: green pulsing "Open to Work" badge
- Mobile: hamburger menu with full-screen overlay
- Active link tracked by scroll position via IntersectionObserver

### 2. Hero (`#hero`)
- Full viewport height, 2-column grid (text left, terminal right)
- **Left column:**
  - Typing label: `> kiran_krishnakumar.init()` (typed char-by-char with random delay)
  - H1: "Data Engineer / Building pipelines / that matter."
  - Description paragraph
  - "View My Work ↓" CTA button
  - Social links (GitHub, LinkedIn)
  - **Pipeline Status Ticker** (see Interactive Elements)
- **Right column:**
  - **Code Terminal** (see Interactive Elements)
- Background: animated grid overlay (`gridMove` keyframe) + radial cyan glow + SVG `feTurbulence` grain texture at 2.8% opacity with `mix-blend-mode: overlay`

### 3. About (`#about`)
- 2-column: photo (380px) + content
- Photo: slight `-2deg` tilt, straightens on hover, cyan border glow
- Photo src: `/assets/img/home-perfil.png` (local file)
- Stats grid: 3 cards — "2+ Years Exp", "6 Certifications", "4+ Technologies" (amber values)
- Education pill: B.Tech, Computer Science · VIT Vellore · 2024

### 4. Metrics Band (`#metrics`)
- 4-column grid of large animated counters
- Values: **12 TB+** (Data Processed), **50+** (Pipelines Built), **99.8%** (Pipeline Uptime), **6** (Certifications)
- Counts up from 0 using `requestAnimationFrame` + ease-out cubic when scrolled into view
- `data-target` and `data-suffix` attributes drive the JS — e.g. `data-target="99" data-suffix=".8%"`

### 5. Experience (`#experience`)
- Timeline with vertical cyan gradient line
- 3 entries:
  1. Senior Associate Engineer — Ascendion (Oct 2025–Present, Bengaluru, Hybrid)
  2. Associate Engineer — Ascendion (Jul 2024–Oct 2025, Chennai, On-site)
  3. Full Stack Engineer Intern — IBS Software (May 2023–Jul 2023, Kochi)
- Each card: amber date, Syne title, role/company, description, tech tags
- **Scrolling Log Stream** at bottom of section (see Interactive Elements)

### 6. Skills (`#skills`)
- 3×2 grid of skill cards with category + JetBrains Mono tags
- Hover: cyan top border sweeps in (`scaleX` from 0 to 1)
- Categories: Programming Languages / Data Engineering / Business Intelligence / Database & Query / Cloud Platforms / AI & Machine Learning

### 7. DAG Visualization (`#dag`)
- SVG-based directed acyclic graph showing a production pipeline
- Nodes: **Blob Storage**, **SQL Server**, **REST API** → **Bronze** → **Silver** → **Gold** + **Semantic Model** → **Power BI**
- Node colours: sources (cyan border), Bronze (amber), Silver (slate), Gold (amber), Semantic (purple), Power BI (green)
- Animated glowing particles flow along each edge using Web Animations API (`stroke-dashoffset`)
- Animation starts only when section scrolls into view (IntersectionObserver)
- SVG viewBox: `0 0 880 290`, `min-width: 600px` with `overflow-x: auto` wrapper

### 8. Certifications (`#certifications`)
- 3-column grid, rendered dynamically from JS array
- Each card has a 3px top border in the issuer's brand colour
- Hover glow computed from brand colour hex → rgba (parsed in JS)
- All 6 certs link to official credential verification URLs
- Cert data lives in the `certifications` JS array — easy to update

**Certifications data (to update):**
```js
const certifications = [
  { title: "...", code: "DP-700", issuer: "Microsoft", color: "#00a4ef", url: "https://..." },
  // ...
]
```
| Cert | Code | Issuer colour |
|---|---|---|
| Fabric Data Engineer Associate | DP-700 | `#00a4ef` |
| Fabric Analytics Engineer Associate | DP-600 | `#00a4ef` |
| Power BI Data Analyst Associate | PL-300 | `#f2c811` |
| Azure Fundamentals | AZ-900 | `#00a4ef` |
| Snowflake SnowPro Associate: Platform | SOC-C01 | `#29b5e8` |
| IBM AI Analyst | AI Analyst | `#1f70c1` |

### 9. Contact (`#contact`)
- 3 link cards: LinkedIn, GitHub, Location (non-clickable)
- No form — deliberately removed. Contact happens via LinkedIn/GitHub only.

### 10. Footer
- Left: "Kiran Krishnakumar · Data Engineer · 2026 · Built with code & caffeine."
- Right: GitHub + LinkedIn icon links

---

## Interactive Elements (Data Engineering Themed)

### 1. Pipeline Status Ticker
**Location:** Hero left column, below social links  
**What it does:** Simulates a live pipeline job running — amber pulsing dot, job name, row counter incrementing, elapsed timer, gradient progress bar. Cycles through 5 fake job names when a job completes.  
**Key IDs:** `#tickerJob`, `#tickerRows`, `#tickerElapsed`, `#tickerFill`  
**To update job names:** Edit the `jobs` array in the ticker IIFE in `<script>`.

### 2. Code Terminal
**Location:** Hero right column (replaced the old static pipeline node list)  
**What it does:** VS Code/Databricks-style terminal card (macOS dots, dark bg) that types out a PySpark/Delta Lake script line by line with syntax highlighting. Loops after a 2.8s pause.  
**Syntax classes:** `.t-kw` (red, keywords), `.t-fn` (purple, functions), `.t-str` (blue, strings), `.t-op` (cyan, operators), `.t-cm` (grey italic, comments), `.t-num` (amber, numbers), `.t-nm` (white, identifiers)  
**Key ID:** `#terminalBody`  
**To change the code:** Edit the `lines` array in the terminal IIFE. Each entry is an HTML string with `<span class="t-*">` tokens — empty string `''` = blank line.

### 3. Animated Metric Counters
**Location:** Metrics section (between About and Experience)  
**What it does:** Numbers count up from 0 using `requestAnimationFrame` with ease-out cubic easing when scrolled into view. Triggers once, then unobserves.  
**To update values:** Change `data-target` and `data-suffix` on `.counter` spans.  
**Example:** `<span class="counter" data-target="99" data-suffix=".8%">0</span>`

### 4. Scrolling Log Stream
**Location:** Bottom of Experience section  
**What it does:** Fake Databricks/ADF pipeline log lines stream in every 1.8s. Starts when visible, stops when off-screen (IntersectionObserver toggle). Keeps max 22 lines, auto-scrolls.  
**Log levels and colours:** INFO (cyan), WARN (amber), DEBUG (purple), ERROR (red)  
**Key ID:** `#logBody`  
**To update log messages:** Edit the `pool` array in the log stream IIFE.

### 5. DAG Flow Animation
**Location:** DAG section (between Skills and Certifications)  
**What it does:** Glowing particles animate along SVG path edges using the Web Animations API. Each edge has a measured `getTotalLength()` and a `stroke-dashoffset` animation that loops infinitely. Starts on scroll into view.  
**Edge colours:** Cyan (source→Bronze, Bronze→Silver), Amber (Silver→Gold/Semantic), Green (Gold/Semantic→Power BI)  
**Key IDs:** `#dagSvg`, `#dagFlows`  
**To add a new node:** Add a `<g class="dag-node">` with a `<rect>` + `<text>`, then add the path to both the static `.dag-path` group and the `edges` array in the DAG IIFE.

---

## Animations & Transitions

| Name | Used on | Description |
|---|---|---|
| `fadeUp` | Hero elements | Opacity 0→1, translateY 20px→0, staggered delays |
| `gridMove` | Hero background | Animated grid pattern scrolls diagonally |
| `pipelineEnter` | (legacy, kept in CSS) | Pipeline node entrance |
| `connectorFlow` | (legacy, kept in CSS) | Dashed connector animation |
| `nodePulse` | Pipeline dots | Scale + glow pulse |
| `pulse-green` | Status dot, log live dot | Green radial pulse |
| `pulse-amber` | Ticker dot | Amber radial pulse |
| `blink` | Terminal cursor | 1s step-end blink |
| `.reveal` | All section content | `translateY(24px)` → `0`, triggered by IntersectionObserver at `threshold: 0.08` |
| `.reveal-delay-1/2/3/4/5` | Stagger children | 100ms–500ms transition-delay |

**Easing used throughout:** `cubic-bezier(0.16, 1, 0.3, 1)` — a fast-out spring curve.

---

## URL Routing (Clean URLs, No Hash)

The site uses the History API for clean URLs — no `#certifications`, just `/certifications`.

**How it works:**
1. All nav/mobile links use `href="/section"` not `href="#section"`
2. Click handler intercepts all `a[href^="/"]` that map to a section ID, prevents default, smooth-scrolls, and calls `history.pushState(null, '', '/section')`
3. Scroll handler calls `history.replaceState` to update URL as user scrolls
4. On direct URL access (e.g. someone lands on `/certifications`), page loads and auto-scrolls to that section
5. `vercel.json` rewrites all paths to `index.html` so Vercel serves the SPA correctly

**To add a new section with clean URL routing:**
- Add `id="your-section"` to the section element
- Add the section ID to the `sections` array in JS: `const sections = ['about', 'experience', ..., 'your-section']`
- Add a nav link: `<a href="/your-section" data-section="your-section">Label</a>`

---

## SEO Setup

### Meta tags in `<head>`
- `<title>` — keyword-rich with role + key technologies
- `<meta description>` — 160 chars, includes years of experience + tech stack
- `<meta keywords>` — data engineering terms
- `<meta robots content="index, follow">`
- `<link rel="canonical">` — `https://kirankrishnakumar.vercel.app/`
- `<link rel="sitemap">` — points to `/sitemap.xml`
- `<meta theme-color content="#050810">` — mobile browser chrome colour

### Open Graph (social sharing previews)
Controls how the link looks when shared on LinkedIn, Slack, WhatsApp, etc.
- `og:title`, `og:description`, `og:url`, `og:image`, `og:type`, `og:locale`
- Image: `https://kirankrishnakumar.vercel.app/assets/img/home-perfil.png`

### Twitter Card
- `twitter:card`, `twitter:title`, `twitter:description`, `twitter:image`

### JSON-LD Structured Data
`<script type="application/ld+json">` with `@type: "Person"` schema including:
- `jobTitle`, `worksFor`, `alumniOf`, `address`, `url`, `sameAs`, `knowsAbout`, `hasCredential`, `description`
This enables rich search results and knowledge panel eligibility on Google.

### Files
| File | Purpose |
|---|---|
| `sitemap.xml` | 6 URLs with priorities — submit to Google Search Console |
| `llms.txt` | Structured markdown for AI crawlers (llmstxt.org spec) |

---

## How to Update Content

### Add a new certification
Find the `certifications` array in `<script>` and add an entry:
```js
{
  title: "New Certification Name",
  code: "EXAM-CODE",
  issuer: "Issuer Name",
  color: "#hexcolor",   // issuer brand colour
  url: "https://credential-verification-url"
}
```

### Update experience
Find the `.timeline` div in the HTML. Each job is a `.timeline-item` > `.timeline-card`. The date uses `.timeline-card__date` (amber, JetBrains Mono).

### Update the metrics numbers
Find the `.metrics-grid` in HTML. Change `data-target` and `data-suffix`:
```html
<span class="counter" data-target="20" data-suffix=" TB+">0</span>
```

### Update the terminal code snippet
Find the `lines` array in the terminal IIFE in `<script>`. Each entry is a pre-tokenized HTML string using `.t-*` span classes for syntax highlighting.

### Update the log stream messages
Find the `pool` array in the log stream IIFE. Each entry: `{ l: 'INFO'|'WARN'|'DEBUG'|'ERROR', m: 'message text' }`.

### Replace the favicon
Three files serve the favicon for maximum browser compatibility:
- `favicon.svg` — used by modern browsers (Chrome 80+, Firefox, Safari 14+). Edit the SVG directly to change colour/text.
- `favicon.ico` — 16/32/48px multi-size ICO, fallback for older browsers. Regenerate with Pillow if changed.
- `assets/img/favicon-32.png` — extra PNG fallback.

To regenerate: run the Pillow script from the conversation (uses `arialbd.ttf` on Windows, dark `#050810` bg, cyan `#00c8ff` text).

### Change profile photo
Replace `/assets/img/home-perfil.png` with a new file and update:
1. The `<img src>` in the About section
2. `og:image` meta tag in `<head>`
3. `twitter:image` meta tag in `<head>`

### Update domain (if custom domain added)
Find and replace `kirankrishnakumar.vercel.app` in:
- `index.html` (6 occurrences — canonical, OG tags, JSON-LD)
- `sitemap.xml` (6 occurrences)
- `llms.txt` (1 occurrence)

---

## Responsive Breakpoints

| Breakpoint | Changes |
|---|---|
| `≤1024px` | Hero goes 1-column; About photo stacks; skills/certs go 2-col; metrics go 2×2 |
| `≤768px` | Nav collapses to hamburger; section padding reduces to 72px; timeline indents reduce |
| `≤480px` | Container padding 16px; hero buttons stack; skills/certs go 1-col; stats stay 3-col |

---

## External Dependencies (CDN, no npm)

```html
<!-- Fonts -->
https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800
  &family=JetBrains+Mono:wght@400;500;600
  &family=DM+Sans:ital,wght@0,300;0,400;0,500;1,400

<!-- Icons -->
https://unpkg.com/lucide@latest/dist/umd/lucide.min.js
```
Lucide icons are loaded but only used in the contact section (inline SVGs are used elsewhere for reliability).

---

## Things to Avoid / Known Decisions

- **Do not add a contact form.** It was deliberately removed — Kiran prefers LinkedIn/GitHub only.
- **Do not change the h1 font weight above 700 or size above `clamp(2.5rem, 6vw, 4.5rem)`** — was tried (800 weight, 6rem) and immediately reverted as it looked bad.
- **Do not use `innerHTML +=` in a loop** for dynamically rendered elements (cert cards were once built this way — causes 6× DOM re-parse and IntersectionObserver timing issues). Always build a full string and assign `innerHTML` once.
- **Cert card glow uses inline `onmouseenter` JS** (not CSS `::before`) because CSS `rgba(var(--cert-color-rgb))` requires a separate RGB variable that was never defined. The hex→RGB parse in JS is the correct approach.
- **The `#hero` section has `padding: 0`** as an explicit override because the generic `section { padding: 100px 0 }` rule would otherwise apply and push hero content down by 100px on top of the 70px nav offset.
- **Metric counters animate once** (observer unobserves after triggering) — they do not reset on re-scroll.
- **Log stream pauses when off-screen** — the `setInterval` is cleared by IntersectionObserver when `.log-stream` leaves the viewport. This avoids background DOM mutation while the user isn't watching.

---

## Quick Reference: Key Element IDs

| ID | Element |
|---|---|
| `heroLabel` | Typed text label in hero |
| `tickerJob` | Job name in pipeline ticker |
| `tickerRows` | Row counter in ticker |
| `tickerElapsed` | Elapsed timer in ticker |
| `tickerFill` | Progress bar fill in ticker |
| `terminalBody` | Code terminal content area |
| `logBody` | Log stream container |
| `dagSvg` | DAG SVG element |
| `dagFlows` | `<g>` inside DAG where animated particles are injected |
| `certsGrid` | Certifications grid (populated from JS array) |
| `scrollProgress` | Top scroll progress bar |
| `nav` | Navigation element |
| `hamburger` | Mobile menu toggle button |
| `mobileMenu` | Mobile nav overlay |
