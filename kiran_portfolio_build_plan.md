# Kiran Krishnakumar — Portfolio Revamp: Claude Code Build Plan

## Mission
Rebuild Kiran's portfolio from scratch as a **single-file** `index.html` (HTML + CSS + vanilla JS, no frameworks, no build step) that can be dropped directly into Netlify. The current site is a generic template. The new one must feel like it was designed *specifically* for a Data Engineer — dark, technical, pipeline-brained, impressive.

---

## Tech Stack Decision
- **Single file**: `index.html` (all CSS in `<style>`, all JS in `<script>`)
- **Fonts**: Load via Google Fonts CDN
  - Display/Headings: `Syne` (bold, geometric, modern)
  - Mono/Code blocks: `JetBrains Mono` (readable, authentic dev feel)
  - Body: `DM Sans` (clean, professional)
- **Icons**: Use inline SVGs or load Lucide icons via CDN
- **No frameworks. No React. No Tailwind.** Pure CSS custom properties + vanilla JS.
- **Deployment**: Drop `index.html` into Netlify (replace existing)

---

## Design System

### Color Palette (CSS Variables)
```css
:root {
  --bg-base: #050810;           /* deep space black */
  --bg-surface: #0c1220;        /* card backgrounds */
  --bg-elevated: #111827;       /* elevated cards */
  --accent-primary: #00c8ff;    /* electric cyan */
  --accent-secondary: #7c3aed; /* deep violet */
  --accent-glow: rgba(0,200,255,0.15);
  --text-primary: #f0f4ff;
  --text-secondary: #8892a4;
  --text-muted: #4a5568;
  --border: rgba(0,200,255,0.12);
  --border-hover: rgba(0,200,255,0.4);
  --gradient-hero: linear-gradient(135deg, #050810 0%, #0c1a2e 50%, #050810 100%);
}
```

### Typography Scale
```css
/* Headings: Syne */
/* Code/labels: JetBrains Mono */
/* Body: DM Sans */

h1 { font: 700 clamp(2.5rem, 6vw, 4.5rem)/1.1 'Syne', sans-serif; }
h2 { font: 700 clamp(1.8rem, 4vw, 2.8rem)/1.2 'Syne', sans-serif; }
.mono { font-family: 'JetBrains Mono', monospace; }
body { font: 400 1rem/1.7 'DM Sans', sans-serif; }
```

### Motion Principles
- All reveals: `opacity 0 → 1` + `translateY(20px) → 0` on scroll (IntersectionObserver)
- Hero entrance: staggered element reveals (100ms delays)
- Hover: `transform: translateY(-3px)` + box-shadow glow on cards
- Background: subtle animated CSS grid lines (no canvas, pure CSS animation)
- Pipeline nodes: pulse animation (scale + opacity loop)

---

## Page Architecture (Single Page, Smooth Scroll)

### Section Order
1. `<nav>` — Fixed top nav with scroll progress bar
2. `#hero` — Full viewport hero
3. `#about` — Bio + stats
4. `#experience` — Career timeline
5. `#skills` — Tech stack grid
6. `#certifications` — Credential badge wall
7. `#contact` — Links + form

---

## Section Specifications

---

### 1. NAV

**Behavior**: Fixed top, `backdrop-filter: blur(12px)`, becomes opaque after 80px scroll. Hamburger on mobile.

**Links**: Home · About · Experience · Skills · Certifications · Contact

**Right side**: "Open to Work" green pulsing dot badge

**Scroll progress bar**: 2px cyan line at top of viewport that fills as user scrolls

```html
<nav>
  <div class="nav__logo">KK<span class="accent">.</span></div>
  <ul class="nav__links">
    <li><a href="#about">About</a></li>
    <li><a href="#experience">Experience</a></li>
    <li><a href="#skills">Skills</a></li>
    <li><a href="#certifications">Certifications</a></li>
    <li><a href="#contact">Contact</a></li>
  </ul>
  <div class="nav__cta">
    <span class="status-dot"></span> Open to Work
  </div>
</nav>
```

---

### 2. HERO SECTION

**Full viewport height**. Background: animated subtle grid using CSS (repeating-linear-gradient at 1px lines, very faint). Add a radial glow centered at bottom-left.

**Layout**: Two column on desktop (text left, visual right), stacked on mobile

**Left column — Text:**
```
[mono label]  > kiran_krishnakumar.init()

[h1]  Data Engineer
      Building pipelines
      that matter.

[body] 2+ years turning raw data into
       high-impact decisions. Microsoft Fabric,
       Snowflake, Databricks, Azure — in production.

[buttons]  [View My Work ↓]   [Download CV]
[links]    GitHub  ·  LinkedIn
```

**Right column — Visual (CSS-only data pipeline diagram):**
Draw a vertical pipeline flow using pure CSS/divs:
- 4-5 "nodes" stacked vertically connected by animated dashed lines
- Node labels: `RAW DATA` → `ETL PIPELINE` → `LAKEHOUSE` → `SEMANTIC MODEL` → `DASHBOARD`
- Each node is a small card with a cyan border and pulsing dot
- Connecting lines animate a "data flowing down" dash animation
- This is done with CSS `border-left: 2px dashed var(--accent-primary)` + `animation: dash 1.5s linear infinite`

**Animations**:
- Hero text staggers in: logo (0ms) → h1 line 1 (100ms) → h1 line 2 (200ms) → body (300ms) → buttons (400ms)
- Pipeline nodes: each node fades in with 150ms stagger + pulse on the dot

---

### 3. ABOUT SECTION

**Layout**: Two columns — Photo left, content right (reverse on mobile)

**Photo**: 
```html
<img src="https://kirankrishnakumar.netlify.app/assets/img/home-perfil.png" 
     alt="Kiran Krishnakumar" class="about__photo">
```
Style: rounded rectangle, cyan border glow, slight tilt (-2deg rotation), drop-shadow

**Right column content**:
```
[mono label]  // about_me

[h2]  Hi, I'm Kiran.

[body paragraph]
Senior Associate Engineer at Ascendion, building 
enterprise-grade data engineering solutions. I specialise
in transforming raw, messy data into scalable pipelines
and decision-ready analytics — using Microsoft Fabric,
Azure Data Factory, Databricks, Snowflake, and dbt Cloud.

I hold 6 industry certifications across Azure, Microsoft 
Fabric, Snowflake, and AI — and I'm always pushing deeper
into the modern data stack.

Currently based in Bengaluru, India.
```

**Stats row** (3 stats in a row, bordered cards):
```
[2+]        [6]           [4+]
Years Exp   Certifications  Technologies
```

**Education badge** (inline, subtle card at bottom of about):
```
🎓  B.Tech, Computer Science · VIT Vellore · 2024 · CGPA 8.23
```

---

### 4. EXPERIENCE SECTION (Timeline)

**Layout**: Vertical timeline. Left side: year labels. Right side: cards. Center: vertical cyan line with dots.

On mobile: single column, line on far left.

**Timeline Items** (chronological, most recent first):

---

**Item 1 — Ascendion (Senior Associate Engineer)**
```
[dot] OCT 2025 – PRESENT
      
      Senior Associate Engineer
      Ascendion · Full-time · Hybrid
      Bengaluru, Karnataka, India

      [tags] Power BI  Microsoft Fabric  Azure  Databricks  Snowflake  Python  SQL
```

---

**Item 2 — Ascendion (Associate Engineer)**
```
[dot] JUL 2024 – OCT 2025

      Associate Engineer  
      Ascendion · Full-time · On-site
      Chennai, Tamil Nadu, India

      Building and optimising data pipelines and ETL workflows across
      Azure Data Factory, Databricks, and Snowflake. Developing Power BI
      dashboards and semantic models for enterprise reporting.

      [tags] Power BI  ADF  SSIS  SQL  Python  ETL
```

---

**Item 3 — IBS Software (Internship)**
```
[dot] MAY 2023 – JUL 2023

      Full Stack Engineer Intern
      IBS Software · Internship · On-site
      Kochi, Kerala, India

      Designed and developed backend services using Java Spring Boot.
      Integrated AI functionalities and built RESTful APIs for secure,
      efficient data processing.

      [tags] Java  Spring Boot  REST APIs  AI Integration
```

**Card style**: Dark surface card, left border in cyan (3px), tag pills styled as `background: rgba(0,200,255,0.1); border: 1px solid var(--border); border-radius: 4px; font-family: JetBrains Mono;`

---

### 5. SKILLS SECTION

**Layout**: 6 category cards in a responsive grid (3 col desktop, 2 col tablet, 1 col mobile)

**Header**:
```
[mono] // tech_stack
[h2]  Tools of the Trade
```

**Each card structure**:
```html
<div class="skill-card">
  <div class="skill-card__icon"><!-- SVG icon --></div>
  <h3 class="skill-card__category">Programming Languages</h3>
  <div class="skill-card__tags">
    <span>Python</span><span>SQL</span><span>ECL</span><span>R</span>
  </div>
</div>
```

**6 Skill Cards — Full Content**:

```
💻 Programming Languages
   Python · SQL · ECL · R · HTML · CSS

⚙️  Data Engineering
   ETL/ELT Pipelines · SSIS · Azure Data Factory
   Databricks · dbt Cloud · Snowflake · Feature Engineering

📊 Business Intelligence
   Power BI · Power BI Service · Paginated Reports
   Tableau · Power Query · Excel

🗄️  Database & Query
   SQL Server · SSMS · Stored Procedures
   Data Modeling · Relational Architecture

☁️  Cloud Platforms
   Microsoft Azure · AWS · Microsoft Fabric
   Snowflake · SnowConvert AI · SnowConvert Migration Accelerator

🤖 AI & Machine Learning
   Machine Learning · Data Science · Generative AI
   Prompt Engineering · EDA · Statistics & Probability
```

**Card hover**: lift 4px + cyan border glow + subtle background lightens

**Visual flourish**: Add a thin cyan top-border that expands on hover (width 0 → 100% transition)

---

### 6. CERTIFICATIONS SECTION

**Layout**: 6 certification cards in a 3×2 grid (2 col on tablet, 1 col mobile)

**Header**:
```
[mono] // credentials
[h2]  Certifications
[subtext] 6 industry-recognised credentials across Azure, 
           Microsoft Fabric, Snowflake & AI
```

**Certification Data (with real URLs)**:

```javascript
const certifications = [
  {
    title: "Microsoft Certified: Fabric Data Engineer Associate",
    code: "DP-700",
    issuer: "Microsoft",
    color: "#00a4ef",
    url: "https://learn.microsoft.com/api/credentials/share/en-us/kirankrishnakumar/274C4C292C847FA?sharingId=10CAE224465F2842"
  },
  {
    title: "Microsoft Certified: Fabric Analytics Engineer Associate",
    code: "DP-600",
    issuer: "Microsoft",
    color: "#00a4ef",
    url: "https://learn.microsoft.com/api/credentials/share/en-us/kirankrishnakumar/53A34D09B971D31?sharingId=10CAE224465F2842"
  },
  {
    title: "Microsoft Certified: Power BI Data Analyst Associate",
    code: "PL-300",
    issuer: "Microsoft",
    color: "#f2c811",
    url: "https://learn.microsoft.com/api/credentials/share/en-us/kirankrishnakumar/C310170C9E936055?sharingId=10CAE224465F2842"
  },
  {
    title: "Microsoft Certified: Azure Fundamentals",
    code: "AZ-900",
    issuer: "Microsoft",
    color: "#00a4ef",
    url: "https://learn.microsoft.com/api/credentials/share/en-us/kirankrishnakumar/F07F617A811D7712?sharingId=10CAE224465F2842"
  },
  {
    title: "Snowflake SnowPro® Associate: Platform",
    code: "SOC-C01",
    issuer: "Snowflake",
    color: "#29b5e8",
    url: "https://achieve.snowflake.com/500b5b0c-d0ae-4f18-beac-7d1bd07eea2d#acc.PH0RJAhN"
  },
  {
    title: "IBM Artificial Intelligence Analyst",
    code: "AI Analyst",
    issuer: "IBM",
    color: "#1f70c1",
    url: "https://courses.ibmcep.cognitiveclass.ai/certificates/1cc736e7060841a0a590ffb59ca77b91"
  }
]
```

**Card design**:
- Dark surface card
- Top: issuer name in mono font (colored to match issuer brand color)
- Middle: certification title (bold Syne)
- Bottom: exam code badge + "View Credential →" link
- Entire card is clickable (opens in new tab)
- Hover: glow border in issuer color, lift animation

---

### 7. CONTACT SECTION

**Background**: Slightly different — add a subtle radial gradient glow in bg

**Header**:
```
[mono] // let's_connect
[h2]  Get in Touch
[body] Whether it's a data challenge, collaboration, or
       just a conversation about the modern data stack.
```

**Two-column layout**:

**Left — Social Links** (stacked link cards):
```
[LinkedIn icon] linkedin.com/in/kiran-k/
                → "Connect on LinkedIn"

[GitHub icon]   github.com/kirankmenon2001  
                → "See my code on GitHub"

[Location pin]  Bengaluru, Karnataka, India
                → "Open to hybrid & remote roles"
```

Each link is a card: dark surface, border, hover lifts + cyan accent

**Right — Contact Form**:
```html
<form>
  <div class="form-group">
    <label>Name</label>
    <input type="text" placeholder="Your name" />
  </div>
  <div class="form-group">
    <label>Email</label>
    <input type="email" placeholder="your@email.com" />
  </div>
  <div class="form-group">
    <label>Message</label>
    <textarea placeholder="What's on your mind?" rows="5"></textarea>
  </div>
  <button type="submit">Send Message →</button>
</form>
```

Form style: dark inputs, cyan focus ring (`outline: 2px solid var(--accent-primary)`), submit button has cyan background + dark text, hover reverses.

**Note**: Form is UI-only (no backend). On submit, show a "Thanks, I'll get back to you!" success message via JS.

---

### 8. FOOTER

Simple, minimal:
```
Kiran Krishnakumar  ·  Data Engineer  ·  2025
Built with code & caffeine.

[GitHub]  [LinkedIn]
```

---

## JavaScript Requirements

```javascript
// 1. Scroll progress bar
window.addEventListener('scroll', () => {
  const pct = window.scrollY / (document.body.scrollHeight - window.innerHeight);
  document.querySelector('.scroll-progress').style.width = `${pct * 100}%`;
});

// 2. Intersection Observer for scroll reveals
const observer = new IntersectionObserver((entries) => {
  entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('visible') });
}, { threshold: 0.1 });
document.querySelectorAll('.reveal').forEach(el => observer.observe(el));

// 3. Active nav link highlight on scroll
// Track which section is in viewport, add .active class to matching nav link

// 4. Nav background on scroll
window.addEventListener('scroll', () => {
  document.querySelector('nav').classList.toggle('scrolled', window.scrollY > 80);
});

// 5. Mobile hamburger menu toggle

// 6. Form submission (mock)
form.addEventListener('submit', e => {
  e.preventDefault();
  // Show success state
});

// 7. Typing effect on hero mono label (optional but nice)
// Type out "> kiran_krishnakumar.init()" character by character
```

---

## CSS Animation Specifications

```css
/* Reveal animation base state */
.reveal {
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}
.reveal.visible {
  opacity: 1;
  transform: translateY(0);
}

/* Stagger children */
.reveal:nth-child(2) { transition-delay: 0.1s; }
.reveal:nth-child(3) { transition-delay: 0.2s; }
/* etc */

/* Pipeline dash animation */
@keyframes dash {
  to { stroke-dashoffset: -20; }
}

/* Pulse for status dot */
@keyframes pulse {
  0%, 100% { opacity: 1; transform: scale(1); }
  50% { opacity: 0.5; transform: scale(1.4); }
}

/* Grid background (hero) */
.hero::before {
  content: '';
  position: absolute;
  inset: 0;
  background-image: 
    linear-gradient(rgba(0,200,255,0.03) 1px, transparent 1px),
    linear-gradient(90deg, rgba(0,200,255,0.03) 1px, transparent 1px);
  background-size: 50px 50px;
  animation: gridMove 20s linear infinite;
}
@keyframes gridMove {
  0% { background-position: 0 0; }
  100% { background-position: 50px 50px; }
}
```

---

## Responsive Breakpoints

```
Mobile:  < 640px   — single column everything, hamburger nav
Tablet:  640–1024px — 2-col grids, timeline left-aligned
Desktop: > 1024px  — full layout as described above
```

---

## File Delivery

Produce a **single file**: `index.html`

All CSS goes inside `<style>` in `<head>`.  
All JS goes inside `<script>` before `</body>`.  
Load fonts via `<link>` from Google Fonts CDN.  
Load Lucide icons via CDN script tag for any icon needs.

The final file should be ready to deploy by dragging into Netlify with zero configuration.

---

## Quality Checklist

Before finishing, verify:
- [ ] All 6 certification links open correctly in new tabs
- [ ] GitHub link: https://github.com/kirankmenon2001
- [ ] LinkedIn link: https://www.linkedin.com/in/kiran-k/
- [ ] Photo loads from: https://kirankrishnakumar.netlify.app/assets/img/home-perfil.png
- [ ] Title is consistently "Data Engineer" everywhere (NOT "Data Analyst")
- [ ] Smooth scroll works on all section anchors
- [ ] Nav active state updates on scroll
- [ ] Mobile hamburger opens/closes
- [ ] No horizontal scroll on mobile
- [ ] Scroll progress bar works
- [ ] Cards have hover animations
- [ ] Reveal animations trigger on scroll
- [ ] Form shows success state on submit
- [ ] All content matches this spec exactly

---

## Content Reference (Final, Verified)

**Full Name**: Kiran Krishnakumar  
**Role**: Data Engineer  
**Current Employer**: Ascendion (Senior Associate Engineer)  
**Location**: Bengaluru, Karnataka, India  
**LinkedIn**: https://www.linkedin.com/in/kiran-k/  
**GitHub**: https://github.com/kirankmenon2001  
**Photo**: https://kirankrishnakumar.netlify.app/assets/img/home-perfil.png  
**Education**: B.Tech CSE · VIT Vellore · 2020–2024 · CGPA 8.23/10  
**Experience**: 2+ years (Ascendion: Jul 2024–Present; IBS Software intern: May–Jul 2023)  
**Certifications**: 6 (see Section 6 above with all URLs)
