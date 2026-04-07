# Wolli Creek Cultural Tours — Website Build Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a complete 5-page static website for Wolli Creek Valley Cultural Guided Tours — a public-facing site that drives bookings and conveys the cultural richness of Jessy Haiyang's Indigenous-led tours.

**Architecture:** Pure HTML/CSS/JS with no build step or framework. Shared CSS variables and component styles are defined once and imported by every page. Shared nav/footer HTML is copy-pasted across pages (no templating engine). One minimal JS file handles the mobile menu toggle and scroll effects.

**Tech Stack:** HTML5, CSS3 (custom properties), vanilla JS — hosted as static files (GitHub Pages / Netlify).

> **Note on TDD:** This is a static site — there is no test runner. "Verify" steps mean opening the file in a browser and checking visually. Use Chrome DevTools device emulation to test mobile.

---

## File Structure

```
/
├── index.html                  # Homepage
├── tours.html                  # Tour detail — full walk breakdown
├── about.html                  # About Jessy
├── book.html                   # Bookings & contact
├── policies.html               # Policies (refunds, safety, WHS)
├── css/
│   ├── base.css                # Variables, reset, typography, utility classes
│   ├── components.css          # Nav, footer, buttons, card, section — shared across all pages
│   ├── home.css                # Homepage-only styles
│   ├── tours.css               # Tours page styles
│   ├── about.css               # About page styles
│   ├── book.css                # Bookings page styles
│   └── policies.css            # Policies page styles
├── js/
│   └── main.js                 # Mobile nav toggle, scroll-reveal, sticky nav shadow
└── images/
    ├── placeholder-hero.jpg    # Placeholder until real photo provided
    └── placeholder-card.jpg    # Placeholder card image
```

---

## Task 1: Project Scaffold + CSS Variables

**Files:**
- Create: `css/base.css`
- Create: `images/` directory (empty, for photos)

- [ ] **Step 1: Create the css directory and base.css**

Create `css/base.css` with the full design token set and reset:

```css
/* ============================================
   BASE.CSS — Variables, reset, typography
   ============================================ */

:root {
  /* Colour palette — Bush & Water */
  --color-forest-dark:   #0D2218;
  --color-forest:        #1B3A2D;
  --color-forest-mid:    #2D6A4F;
  --color-sage:          #52B788;
  --color-sage-light:    #D4E9C2;
  --color-cream:         #f5f0e8;
  --color-ochre:         #8B6914;
  --color-wetland:       #2D6A8F;
  --color-text:          #1B3A2D;
  --color-text-muted:    #4a6b5a;

  /* Typography */
  --font-serif:  Georgia, 'Times New Roman', serif;
  --font-sans:   -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;

  /* Spacing */
  --space-xs:  8px;
  --space-sm:  16px;
  --space-md:  32px;
  --space-lg:  64px;
  --space-xl:  96px;

  /* Layout */
  --max-width: 1200px;
  --nav-height: 60px;
}

/* Reset */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body {
  font-family: var(--font-serif);
  background: var(--color-cream);
  color: var(--color-text);
  line-height: 1.6;
}
img { max-width: 100%; display: block; }
a { color: inherit; text-decoration: none; }

/* Typography scale */
h1 { font-size: clamp(2rem, 5vw, 4rem); font-weight: 400; line-height: 1.15; font-style: italic; }
h1 strong { font-style: normal; font-weight: 700; }
h2 { font-size: clamp(1.6rem, 3vw, 2.4rem); font-weight: 400; }
h3 { font-size: 1.1rem; font-weight: 600; }
p  { font-family: var(--font-sans); font-size: 14px; line-height: 1.75; color: var(--color-text-muted); }

/* Utility */
.label {
  font-family: var(--font-sans);
  font-size: 10px;
  letter-spacing: 3px;
  text-transform: uppercase;
  color: var(--color-sage);
}
.container { max-width: var(--max-width); margin: 0 auto; padding: 0 var(--space-md); }
.section { padding: var(--space-lg) var(--space-md); max-width: var(--max-width); margin: 0 auto; }
```

- [ ] **Step 2: Create images directory**

```bash
mkdir images
```

Then download or create two solid-colour placeholder images (or just leave the directory empty — the CSS will fall back to gradient backgrounds where no image is set).

- [ ] **Step 3: Verify base.css is valid**

Open `css/base.css` in a browser via `File > Open` — it should display as plain text with no errors. Alternatively, paste into [jigsaw.w3.org/css-validator](https://jigsaw.w3.org/css-validator/) and confirm 0 errors.

- [ ] **Step 4: Commit**

```bash
git add css/base.css
git commit -m "feat: add base CSS variables, reset, and typography"
```

---

## Task 2: Shared Component Styles (Nav + Footer + Buttons + Cards)

**Files:**
- Create: `css/components.css`

- [ ] **Step 1: Create css/components.css**

```css
/* ============================================
   COMPONENTS.CSS — Nav, footer, buttons, cards
   ============================================ */

/* ---- NAV ---- */
.site-nav {
  background: var(--color-forest);
  padding: 0 var(--space-md);
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: var(--nav-height);
  position: sticky;
  top: 0;
  z-index: 100;
  transition: box-shadow 0.2s;
}
.site-nav.scrolled { box-shadow: 0 2px 16px rgba(0,0,0,0.3); }

.nav-logo {
  color: var(--color-sage-light);
  font-family: var(--font-sans);
  font-size: 13px;
  font-weight: 700;
  letter-spacing: 2px;
  text-transform: uppercase;
  text-decoration: none;
}
.nav-logo span { color: var(--color-sage); }

.nav-links {
  display: flex;
  gap: var(--space-md);
  list-style: none;
}
.nav-links a {
  color: rgba(212,233,194,0.7);
  font-family: var(--font-sans);
  font-size: 12px;
  letter-spacing: 1px;
  text-transform: uppercase;
  transition: color 0.15s;
}
.nav-links a:hover,
.nav-links a.active { color: var(--color-sage-light); }

.nav-cta {
  background: var(--color-sage);
  color: var(--color-forest-dark);
  padding: 8px 18px;
  border-radius: 4px;
  font-family: var(--font-sans);
  font-size: 12px;
  font-weight: 700;
  letter-spacing: 1px;
  text-transform: uppercase;
  text-decoration: none;
  transition: background 0.15s;
}
.nav-cta:hover { background: var(--color-sage-light); }

/* Mobile nav toggle */
.nav-toggle {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px;
}
.nav-toggle span {
  display: block;
  width: 22px;
  height: 2px;
  background: var(--color-sage-light);
  margin: 5px 0;
  transition: 0.3s;
}

@media (max-width: 768px) {
  .nav-links { display: none; flex-direction: column; gap: var(--space-sm); }
  .nav-links.open { display: flex; position: absolute; top: var(--nav-height); left: 0; right: 0; background: var(--color-forest); padding: var(--space-sm) var(--space-md); }
  .nav-toggle { display: block; }
  .nav-cta { display: none; }
}

/* ---- FOOTER ---- */
.site-footer {
  background: var(--color-forest-dark);
  padding: var(--space-md) var(--space-md);
  text-align: center;
}
.site-footer p {
  color: rgba(212,233,194,0.4);
  font-size: 12px;
  margin-bottom: 4px;
}
.site-footer a { color: rgba(212,233,194,0.55); }
.site-footer a:hover { color: var(--color-sage-light); }

/* ---- BUTTONS ---- */
.btn {
  display: inline-block;
  padding: 14px 28px;
  border-radius: 4px;
  font-family: var(--font-sans);
  font-size: 13px;
  font-weight: 700;
  letter-spacing: 1px;
  text-transform: uppercase;
  cursor: pointer;
  border: none;
  text-decoration: none;
  transition: background 0.15s, color 0.15s;
}
.btn-primary {
  background: var(--color-sage);
  color: var(--color-forest-dark);
}
.btn-primary:hover { background: var(--color-sage-light); }

.btn-ghost {
  background: transparent;
  color: var(--color-sage-light);
  border: 1px solid rgba(212,233,194,0.4);
}
.btn-ghost:hover { border-color: var(--color-sage-light); color: #fff; }

.btn-ghost-dark {
  background: transparent;
  color: var(--color-forest-dark);
  border: 1px solid rgba(13,34,24,0.3);
}
.btn-ghost-dark:hover { border-color: var(--color-forest-dark); }

/* ---- HIGHLIGHT CARD ---- */
.card {
  background: #fff;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 12px rgba(27,58,45,0.08);
  transition: transform 0.2s;
}
.card:hover { transform: translateY(-4px); }

.card-img {
  height: 160px;
  display: flex;
  align-items: flex-end;
  padding: var(--space-sm);
}
.card-tag {
  background: rgba(13,34,24,0.75);
  color: var(--color-sage-light);
  backdrop-filter: blur(4px);
  font-family: var(--font-sans);
  font-size: 9px;
  letter-spacing: 2px;
  text-transform: uppercase;
  padding: 4px 10px;
  border-radius: 3px;
}
.card-accent { height: 3px; border-radius: 0; }
.card-body { padding: var(--space-sm); }
.card-body h4 { font-size: 1rem; color: var(--color-text); margin-bottom: 6px; font-family: var(--font-serif); }
.card-body p { font-size: 12px; line-height: 1.6; }
.card-meta { font-family: var(--font-sans); font-size: 10px; color: var(--color-sage); font-weight: 600; letter-spacing: 1px; margin-top: 10px; }

/* ---- DIVIDER ---- */
.divider { height: 1px; background: rgba(82,183,136,0.15); margin: 0 var(--space-md); }

/* ---- PAGE HERO (inner pages) ---- */
.page-hero {
  background: linear-gradient(160deg, var(--color-forest-dark) 0%, var(--color-forest) 60%, var(--color-forest-mid) 100%);
  padding: var(--space-xl) var(--space-md) var(--space-lg);
  text-align: center;
}
.page-hero .label { margin-bottom: var(--space-sm); display: block; }
.page-hero h1 { color: var(--color-sage-light); margin-bottom: var(--space-sm); }
.page-hero p { color: rgba(212,233,194,0.7); max-width: 560px; margin: 0 auto; }
```

- [ ] **Step 2: Verify in browser**

Create a temporary `test.html` in the project root:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Component Test</title>
  <link rel="stylesheet" href="css/base.css">
  <link rel="stylesheet" href="css/components.css">
</head>
<body>
  <nav class="site-nav">
    <a href="/" class="nav-logo">Wolli Creek <span>Cultural Tours</span></a>
    <ul class="nav-links">
      <li><a href="tours.html">Tours</a></li>
      <li><a href="about.html">About</a></li>
      <li><a href="policies.html">Policies</a></li>
      <li><a href="book.html">Contact</a></li>
    </ul>
    <a href="book.html" class="nav-cta">Book Now</a>
    <button class="nav-toggle" aria-label="Menu"><span></span><span></span><span></span></button>
  </nav>
  <div style="padding:40px;">
    <span class="label">Tour Highlights</span>
    <h2>What You'll Experience</h2>
    <div style="display:flex;gap:16px;margin-top:20px;flex-wrap:wrap;">
      <div class="card" style="width:280px;">
        <div class="card-img" style="background:linear-gradient(135deg,#1B3A2D,#2D6A4F);">
          <span class="card-tag">Cultural Heritage</span>
        </div>
        <div class="card-accent" style="background:#52B788;"></div>
        <div class="card-body">
          <h4>Ancient Cave Paintings</h4>
          <p>27 hand prints from Bidjigal and Dharawal artists — up to 6,500 years old.</p>
          <div class="card-meta">Stop 4 · 10:45 AM</div>
        </div>
      </div>
    </div>
    <div style="margin-top:20px;display:flex;gap:12px;flex-wrap:wrap;">
      <a href="#" class="btn btn-primary">Book a Tour</a>
      <a href="#" class="btn btn-ghost">Explore the Walk →</a>
    </div>
  </div>
  <footer class="site-footer">
    <p>Wolli Creek Valley Cultural Tours · Bidjigal & Dharawal Country</p>
    <p><a href="mailto:jessyhaiyang@gmail.com">jessyhaiyang@gmail.com</a> · 0491 701 488</p>
  </footer>
</body>
</html>
```

Open in browser. Check: nav is dark green, sticky; card renders with tag and accent line; buttons styled correctly; footer dark. Resize to mobile — nav links should hide and toggle button appear.

- [ ] **Step 3: Delete test.html**

```bash
rm test.html
```

- [ ] **Step 4: Commit**

```bash
git add css/components.css
git commit -m "feat: add shared component styles (nav, footer, buttons, cards)"
```

---

## Task 3: JS — Mobile Nav + Scroll Effects

**Files:**
- Create: `js/main.js`

- [ ] **Step 1: Create js/main.js**

```js
// main.js — Mobile nav toggle + sticky nav shadow + scroll-reveal

document.addEventListener('DOMContentLoaded', () => {

  // --- Mobile nav toggle ---
  const nav = document.querySelector('.site-nav');
  const toggle = document.querySelector('.nav-toggle');
  const navLinks = document.querySelector('.nav-links');

  if (toggle && navLinks) {
    toggle.addEventListener('click', () => {
      navLinks.classList.toggle('open');
      const expanded = navLinks.classList.contains('open');
      toggle.setAttribute('aria-expanded', expanded);
    });
  }

  // Close mobile nav when a link is clicked
  if (navLinks) {
    navLinks.querySelectorAll('a').forEach(link => {
      link.addEventListener('click', () => navLinks.classList.remove('open'));
    });
  }

  // --- Sticky nav shadow on scroll ---
  if (nav) {
    window.addEventListener('scroll', () => {
      nav.classList.toggle('scrolled', window.scrollY > 10);
    }, { passive: true });
  }

  // --- Active nav link ---
  const currentPath = window.location.pathname.split('/').pop() || 'index.html';
  document.querySelectorAll('.nav-links a').forEach(link => {
    const href = link.getAttribute('href');
    if (href === currentPath || (currentPath === '' && href === 'index.html')) {
      link.classList.add('active');
    }
  });

  // --- Scroll-reveal for cards ---
  const revealEls = document.querySelectorAll('.reveal');
  if ('IntersectionObserver' in window && revealEls.length > 0) {
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(el => {
        if (el.isIntersecting) {
          el.target.classList.add('revealed');
          observer.unobserve(el.target);
        }
      });
    }, { threshold: 0.1 });
    revealEls.forEach(el => observer.observe(el));
  } else {
    // Fallback: show everything
    revealEls.forEach(el => el.classList.add('revealed'));
  }

});
```

- [ ] **Step 2: Add reveal CSS to base.css**

Open `css/base.css` and append:

```css
/* Scroll-reveal */
.reveal { opacity: 0; transform: translateY(20px); transition: opacity 0.5s ease, transform 0.5s ease; }
.reveal.revealed { opacity: 1; transform: none; }
```

- [ ] **Step 3: Commit**

```bash
git add js/main.js css/base.css
git commit -m "feat: add mobile nav toggle and scroll-reveal JS"
```

---

## Task 4: Homepage (index.html)

**Files:**
- Create: `index.html`
- Create: `css/home.css`

The homepage layout is already fully designed — adapt from `.superpowers/brainstorm/2753-1775531004/content/homepage-layout.html`.

- [ ] **Step 1: Create css/home.css**

```css
/* ============================================
   HOME.CSS — Homepage-specific styles
   ============================================ */

/* ---- HERO ---- */
.home-hero {
  min-height: 88vh;
  background: linear-gradient(160deg, var(--color-forest-dark) 0%, var(--color-forest) 45%, var(--color-forest-mid) 80%, #3a7a5f 100%);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 60px var(--space-md);
  position: relative;
  overflow: hidden;
}
.home-hero::before {
  content: '';
  position: absolute;
  inset: 0;
  background: radial-gradient(ellipse at 60% 40%, rgba(82,183,136,0.12) 0%, transparent 65%);
  pointer-events: none;
}
.hero-badge {
  background: rgba(82,183,136,0.2);
  border: 1px solid rgba(82,183,136,0.4);
  color: var(--color-sage);
  font-family: var(--font-sans);
  font-size: 10px;
  letter-spacing: 3px;
  text-transform: uppercase;
  padding: 6px 16px;
  border-radius: 20px;
  margin-bottom: 28px;
  display: inline-block;
}
.home-hero h1 { color: var(--color-sage-light); max-width: 780px; margin-bottom: 8px; }
.hero-subtitle {
  font-family: var(--font-sans);
  color: rgba(212,233,194,0.7);
  font-size: 14px;
  letter-spacing: 1px;
  margin-bottom: 20px;
  text-transform: uppercase;
}
.hero-quote {
  max-width: 560px;
  color: rgba(212,233,194,0.85);
  font-size: 1.05rem;
  line-height: 1.7;
  margin-bottom: 36px;
  border-left: 3px solid var(--color-sage);
  padding-left: 20px;
  text-align: left;
}
.hero-quote cite { font-size: 0.85em; color: rgba(212,233,194,0.5); font-style: normal; display: block; margin-top: 8px; }
.hero-buttons { display: flex; gap: var(--space-sm); flex-wrap: wrap; justify-content: center; }
.hero-scroll {
  position: absolute;
  bottom: 24px;
  left: 50%;
  transform: translateX(-50%);
  color: rgba(212,233,194,0.35);
  font-family: var(--font-sans);
  font-size: 10px;
  letter-spacing: 2px;
  text-transform: uppercase;
}

/* ---- STATS BAND ---- */
.stats-band {
  background: var(--color-forest-mid);
  color: var(--color-sage-light);
  padding: 28px var(--space-md);
  display: flex;
  gap: 48px;
  justify-content: center;
  flex-wrap: wrap;
  text-align: center;
}
.stat { font-family: var(--font-sans); }
.stat strong { display: block; font-size: 1.6rem; color: #fff; }
.stat span { font-size: 11px; letter-spacing: 1px; text-transform: uppercase; opacity: 0.7; }

/* ---- HIGHLIGHTS SECTION ---- */
.highlights-section { background: var(--color-cream); padding: var(--space-lg) 0; }
.card-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  margin-top: var(--space-md);
}
@media (max-width: 900px) { .card-grid { grid-template-columns: repeat(2, 1fr); } }
@media (max-width: 560px) { .card-grid { grid-template-columns: 1fr; } }

/* ---- ABOUT STRIP ---- */
.about-strip {
  background: var(--color-forest);
  padding: var(--space-lg) var(--space-md);
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 60px;
  align-items: center;
}
.about-strip .about-text .label { color: var(--color-sage); }
.about-strip .about-text h2 { color: var(--color-sage-light); margin: 8px 0 16px; }
.about-strip .about-text p { color: rgba(212,233,194,0.75); }
.about-img {
  border-radius: 8px;
  overflow: hidden;
  height: 300px;
  background: linear-gradient(135deg, var(--color-forest-mid), var(--color-sage));
  display: flex;
  align-items: center;
  justify-content: center;
  color: rgba(212,233,194,0.4);
  font-family: var(--font-sans);
  font-size: 12px;
  letter-spacing: 1px;
}
.about-img img { width: 100%; height: 100%; object-fit: cover; }
@media (max-width: 768px) {
  .about-strip { grid-template-columns: 1fr; }
  .about-img { height: 220px; }
}

/* ---- CTA SECTION ---- */
.cta-section {
  background: linear-gradient(135deg, var(--color-sage) 0%, var(--color-forest-mid) 100%);
  padding: 80px var(--space-md);
  text-align: center;
}
.cta-section .label { color: var(--color-forest-dark); }
.cta-section h2 { color: var(--color-forest-dark); margin: 8px 0 16px; }
.cta-section p { color: rgba(13,34,24,0.7); margin-bottom: 32px; }
.cta-buttons { display: flex; gap: 12px; justify-content: center; flex-wrap: wrap; }
```

- [ ] **Step 2: Create index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Wolli Creek Valley Cultural Guided Tours — walk the valley and hear the stories of Country with Jessy Haiyang on Bidjigal and Dharawal Country, Inner West Sydney.">
  <title>Wolli Creek Cultural Tours — Walk the Valley</title>
  <link rel="stylesheet" href="css/base.css">
  <link rel="stylesheet" href="css/components.css">
  <link rel="stylesheet" href="css/home.css">
</head>
<body>

  <!-- NAV -->
  <nav class="site-nav">
    <a href="index.html" class="nav-logo">Wolli Creek <span>Cultural Tours</span></a>
    <ul class="nav-links">
      <li><a href="tours.html">Tours</a></li>
      <li><a href="about.html">About</a></li>
      <li><a href="policies.html">Policies</a></li>
      <li><a href="book.html">Contact</a></li>
    </ul>
    <a href="book.html" class="nav-cta">Book Now</a>
    <button class="nav-toggle" aria-label="Open menu" aria-expanded="false">
      <span></span><span></span><span></span>
    </button>
  </nav>

  <!-- HERO -->
  <section class="home-hero">
    <span class="hero-badge">Bidjigal &amp; Dharawal Country · Inner West Sydney</span>
    <h1><strong>Walk the Valley.</strong><br>Hear the Stories of Country.</h1>
    <p class="hero-subtitle">Wolli Creek Valley · Est. 2025</p>
    <blockquote class="hero-quote">
      "I would like to create the opportunity for Sydney to showcase the rich natural history
      of the Eora nations people on Country — sharing bush foods, ancient cave paintings,
      and the living knowledge of our ancestors."
      <cite>— Jessy Haiyang, Guide &amp; Founder</cite>
    </blockquote>
    <div class="hero-buttons">
      <a href="book.html" class="btn btn-primary">Book a Tour</a>
      <a href="tours.html" class="btn btn-ghost">Explore the Walk →</a>
    </div>
    <div class="hero-scroll">↓ &nbsp; scroll to explore</div>
  </section>

  <!-- STATS BAND -->
  <div class="stats-band">
    <div class="stat"><strong>2.75 hrs</strong><span>Guided Walk</span></div>
    <div class="stat"><strong>10</strong><span>Landmark Stops</span></div>
    <div class="stat"><strong>6,500 yrs</strong><span>Ancient Cave Paintings</span></div>
    <div class="stat"><strong>100+</strong><span>Native Bird Species</span></div>
    <div class="stat"><strong>3</strong><span>Start/End Points</span></div>
  </div>

  <!-- HIGHLIGHTS -->
  <section class="highlights-section">
    <div class="section">
      <span class="label">Tour Highlights</span>
      <h2>What You'll Experience</h2>
      <p style="max-width:560px;margin-top:8px;">Ten landmark stops through the Wolli Creek Valley — each with its own story, ecology, and connection to Country.</p>

      <div class="card-grid">

        <div class="card reveal">
          <div class="card-img" style="background:linear-gradient(135deg,#1B3A2D,#2D6A4F);">
            <span class="card-tag">Cultural Heritage</span>
          </div>
          <div class="card-accent" style="background:#52B788;"></div>
          <div class="card-body">
            <h4>Ancient Cave Paintings</h4>
            <p>27 hand prints &amp; 2 feet prints from the Bidjigal and Dharawal artists — up to 6,500 years old.</p>
            <div class="card-meta">Stop 4 · 10:45 AM</div>
          </div>
        </div>

        <div class="card reveal">
          <div class="card-img" style="background:linear-gradient(135deg,#3a5a3a,#52B788);">
            <span class="card-tag">Bush Foods</span>
          </div>
          <div class="card-accent" style="background:#2D6A4F;"></div>
          <div class="card-body">
            <h4>Bush Foods &amp; Medicinal Plants</h4>
            <p>Native edible plants, medicines and trees of cultural significance — used for tools, canoes, and healing.</p>
            <div class="card-meta">Stops 3 &amp; 6</div>
          </div>
        </div>

        <div class="card reveal">
          <div class="card-img" style="background:linear-gradient(135deg,#1a2a1a,#3a6a4a);">
            <span class="card-tag">Wildlife</span>
          </div>
          <div class="card-accent" style="background:#52B788;"></div>
          <div class="card-body">
            <h4>Grey-headed Flying-fox Colony</h4>
            <p>Australia's largest bat — a colony of national importance roosting in the Wolli Creek Valley.</p>
            <div class="card-meta">Stop 8 · 11:45 AM</div>
          </div>
        </div>

        <div class="card reveal">
          <div class="card-img" style="background:linear-gradient(135deg,#5a3a1a,#8B6914);">
            <span class="card-tag">Geology</span>
          </div>
          <div class="card-accent" style="background:#8B6914;"></div>
          <div class="card-body">
            <h4>Ochre Rock Mine</h4>
            <p>Yellow, white and orange sandstone ochre — the very pigments used to create the valley's cave paintings.</p>
            <div class="card-meta">Stop 7 · 11:30 AM</div>
          </div>
        </div>

        <div class="card reveal">
          <div class="card-img" style="background:linear-gradient(135deg,#1a3a4a,#2D6A8F);">
            <span class="card-tag">Wetlands</span>
          </div>
          <div class="card-accent" style="background:#2D6A8F;"></div>
          <div class="card-body">
            <h4>Tidal Salt Water Marshlands</h4>
            <p>Ancient tribal meeting point at Wolli Creek and Cooks River — once teeming with shellfish.</p>
            <div class="card-meta">Stop 3 · 10:30 AM</div>
          </div>
        </div>

        <div class="card reveal">
          <div class="card-img" style="background:linear-gradient(135deg,#2a3a1a,#4a6a2a);">
            <span class="card-tag">Conservation</span>
          </div>
          <div class="card-accent" style="background:#52B788;"></div>
          <div class="card-body">
            <h4>Bush Regeneration Sites</h4>
            <p>See active native planting and weed removal — 5% of tour income goes directly to Valley regeneration.</p>
            <div class="card-meta">Stops 3, 6 &amp; throughout</div>
          </div>
        </div>

      </div>

      <div style="text-align:center;margin-top:40px;">
        <a href="tours.html" class="btn btn-primary">See All 10 Stops →</a>
      </div>
    </div>
  </section>

  <!-- ABOUT STRIP -->
  <section class="about-strip">
    <div class="about-text">
      <span class="label">About Your Guide</span>
      <h2>Jessy Haiyang</h2>
      <p>Descendant of the Pitjara and Iningai people of central Queensland, Jessy has lived in Marrickville for six years and based her Diploma of Conservation and Land Management on the Wolli Creek Valley itself. Her deep knowledge of flora, fauna, and the cultural history of the Eora nations people shapes every step of the tour.</p>
      <br>
      <a href="about.html" class="btn btn-primary" style="margin-top:8px;">Read Jessy's Story →</a>
    </div>
    <div class="about-img">
      <!-- Replace with: <img src="images/jessy.jpg" alt="Jessy Haiyang, tour guide"> -->
      Photo coming soon
    </div>
  </section>

  <!-- CTA -->
  <section class="cta-section">
    <span class="label">Ready to Walk Country?</span>
    <h2>Join a Guided Tour</h2>
    <p>Tours depart from Tempe Station Carpark · Small groups · 2.75 hours · All abilities welcome</p>
    <div class="cta-buttons">
      <a href="book.html" class="btn btn-primary" style="background:#0D2218;color:#D4E9C2;">Book Now</a>
      <a href="mailto:jessyhaiyang@gmail.com" class="btn btn-ghost-dark">Enquire via Email</a>
      <a href="tel:0491701488" class="btn btn-ghost-dark">Call Jessy</a>
    </div>
  </section>

  <!-- FOOTER -->
  <footer class="site-footer">
    <p>Wolli Creek Valley Cultural Tours · Bidjigal &amp; Dharawal Country · Inner West Sydney</p>
    <p><a href="mailto:jessyhaiyang@gmail.com">jessyhaiyang@gmail.com</a> · <a href="tel:0491701488">0491 701 488</a></p>
  </footer>

  <script src="js/main.js"></script>
</body>
</html>
```

- [ ] **Step 3: Verify in browser**

Open `index.html` in a browser. Check:
- Hero section fills viewport, quote visible, buttons clickable
- Stats band is green
- 6 highlight cards in 3-column grid, hover lifts the card
- About strip: 2 columns, dark green background, photo placeholder
- CTA section: green gradient
- Footer: dark, email and phone correct
- Resize to mobile (< 768px): cards go to 1 column, nav toggle appears

- [ ] **Step 4: Commit**

```bash
git add index.html css/home.css
git commit -m "feat: build homepage with hero, highlights grid, about strip, CTA"
```

---

## Task 5: Tour Detail Page (tours.html)

**Files:**
- Create: `tours.html`
- Create: `css/tours.css`

- [ ] **Step 1: Create css/tours.css**

```css
/* ============================================
   TOURS.CSS — Tour detail page
   ============================================ */

/* Quick facts bar */
.facts-bar {
  background: var(--color-forest);
  padding: 20px var(--space-md);
  display: flex;
  gap: 32px;
  justify-content: center;
  flex-wrap: wrap;
  text-align: center;
}
.fact { font-family: var(--font-sans); }
.fact strong { display: block; font-size: 1rem; color: var(--color-sage-light); font-weight: 700; }
.fact span { font-size: 10px; letter-spacing: 1px; text-transform: uppercase; color: rgba(212,233,194,0.5); }

/* Tour narrative */
.tour-narrative { background: var(--color-cream); padding: var(--space-lg) 0; }
.tour-narrative .container p { max-width: 720px; margin-bottom: var(--space-sm); font-size: 15px; line-height: 1.85; color: var(--color-text-muted); }
.tour-narrative .container h2 { margin-bottom: var(--space-sm); }

/* Stop timeline */
.stops-section { background: #fff; padding: var(--space-lg) 0; }
.stops-section h2 { margin-bottom: var(--space-md); }

.stop {
  display: grid;
  grid-template-columns: 60px 1fr;
  gap: var(--space-sm);
  padding: var(--space-sm) 0;
  border-bottom: 1px solid rgba(82,183,136,0.12);
  align-items: start;
}
.stop:last-child { border-bottom: none; }

.stop-number {
  width: 44px;
  height: 44px;
  border-radius: 50%;
  background: var(--color-forest-mid);
  color: #fff;
  font-family: var(--font-sans);
  font-size: 14px;
  font-weight: 700;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}
.stop-content h3 { font-size: 1rem; color: var(--color-text); margin-bottom: 4px; }
.stop-content p { font-size: 13px; line-height: 1.65; margin-bottom: 6px; }
.stop-tags { display: flex; gap: 8px; flex-wrap: wrap; }
.stop-tag {
  font-family: var(--font-sans);
  font-size: 9px;
  letter-spacing: 2px;
  text-transform: uppercase;
  padding: 3px 8px;
  border-radius: 3px;
  background: var(--color-cream);
  color: var(--color-text-muted);
}
.stop-time { font-family: var(--font-sans); font-size: 10px; color: var(--color-sage); font-weight: 600; letter-spacing: 1px; }

/* Practical info */
.practical-section { background: var(--color-cream); padding: var(--space-lg) 0; }
.practical-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: var(--space-md);
  margin-top: var(--space-md);
}
@media (max-width: 768px) { .practical-grid { grid-template-columns: 1fr; } }
.practical-item h4 { font-size: 0.9rem; color: var(--color-text); margin-bottom: 8px; font-family: var(--font-sans); font-weight: 700; text-transform: uppercase; letter-spacing: 1px; font-size: 11px; }
.practical-item p { font-size: 13px; }

/* Inline CTA */
.inline-cta {
  background: var(--color-forest);
  padding: var(--space-lg) var(--space-md);
  text-align: center;
}
.inline-cta h2 { color: var(--color-sage-light); margin-bottom: 16px; }
.inline-cta .btn-primary { background: var(--color-sage); color: var(--color-forest-dark); }
```

- [ ] **Step 2: Create tours.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="The Wolli Creek Valley Cultural Walk — 2.75 hours, 10 landmark stops, bush foods, cave paintings, and living stories of Country.">
  <title>The Tour — Wolli Creek Cultural Tours</title>
  <link rel="stylesheet" href="css/base.css">
  <link rel="stylesheet" href="css/components.css">
  <link rel="stylesheet" href="css/tours.css">
</head>
<body>

  <nav class="site-nav">
    <a href="index.html" class="nav-logo">Wolli Creek <span>Cultural Tours</span></a>
    <ul class="nav-links">
      <li><a href="tours.html" class="active">Tours</a></li>
      <li><a href="about.html">About</a></li>
      <li><a href="policies.html">Policies</a></li>
      <li><a href="book.html">Contact</a></li>
    </ul>
    <a href="book.html" class="nav-cta">Book Now</a>
    <button class="nav-toggle" aria-label="Open menu" aria-expanded="false">
      <span></span><span></span><span></span>
    </button>
  </nav>

  <!-- PAGE HERO -->
  <header class="page-hero">
    <span class="label">The Walk</span>
    <h1><strong>Wolli Creek Valley</strong><br>Cultural Guided Tour</h1>
    <p>2.75 hours · 10 landmark stops · Bidjigal &amp; Dharawal Country · Inner West Sydney</p>
  </header>

  <!-- QUICK FACTS -->
  <div class="facts-bar">
    <div class="fact"><strong>2.75 hrs</strong><span>Duration</span></div>
    <div class="fact"><strong>~4 km</strong><span>Distance</span></div>
    <div class="fact"><strong>Easy–Moderate</strong><span>Difficulty</span></div>
    <div class="fact"><strong>Max 15</strong><span>Group Size</span></div>
    <div class="fact"><strong>TBC</strong><span>Per Person</span></div>
    <div class="fact"><strong>Tempe Station</strong><span>Meeting Point</span></div>
  </div>

  <!-- TOUR NARRATIVE -->
  <section class="tour-narrative">
    <div class="container">
      <span class="label">The Experience</span>
      <h2>Walking Country with Jessy</h2>
      <p>The Wolli Creek Valley Cultural Guided Tour is a 2.75-hour walking journey through one of Sydney's most extraordinary hidden corridors — a living bushland reserve in the heart of the Inner West, where Bidjigal and Dharawal people have lived, gathered, and left their mark for thousands of years.</p>
      <p>Your guide Jessy Haiyang — descendant of the Pitjara and Iningai peoples of central Queensland — has spent years walking this valley, studying its ecology and deepening her understanding of the cultural knowledge embedded in every rock, plant, and waterway. On this tour, she shares what she's learned: not as a museum exhibit, but as living knowledge, still relevant and still present in the landscape today.</p>
      <p>You'll taste bush foods, see ancient cave paintings, walk beside a tidal saltwater marsh, and witness an active flying-fox colony of national significance. You'll also see bush regeneration in action — a reminder that caring for Country is ongoing work. Five percent of every tour booking goes directly to supporting revegetation in the valley.</p>
    </div>
  </section>

  <!-- STOPS -->
  <section class="stops-section">
    <div class="container">
      <span class="label">The Route</span>
      <h2>10 Landmark Stops</h2>

      <div class="stop">
        <div class="stop-number">1</div>
        <div class="stop-content">
          <h3>Tempe Station Carpark — Welcome to Country</h3>
          <p>Meet Jessy at the carpark. The tour begins with a Welcome to Country, an introduction to the Bidjigal and Dharawal peoples whose Country we walk on, and an overview of the valley ahead.</p>
          <div class="stop-tags"><span class="stop-tag">Cultural</span></div>
          <div class="stop-time">10:00 AM · Meeting Point</div>
        </div>
      </div>

      <div class="stop">
        <div class="stop-number">2</div>
        <div class="stop-content">
          <h3>Valley Entrance — Ecology Overview</h3>
          <p>Enter the Wolli Creek Valley and take in its scale. Jessy introduces the valley's ecology — the creek corridor, its bird species, and the significance of this landscape as a refuge within an urban environment.</p>
          <div class="stop-tags"><span class="stop-tag">Ecology</span></div>
          <div class="stop-time">10:15 AM</div>
        </div>
      </div>

      <div class="stop">
        <div class="stop-number">3</div>
        <div class="stop-content">
          <h3>Tidal Saltwater Marshlands</h3>
          <p>The ancient tribal meeting point at the confluence of Wolli Creek and Cooks River. Once teeming with shellfish, oysters, and mullet — a vital food source. Jessy explains the significance of waterways in Eora culture and the ecology of the surviving marshland.</p>
          <div class="stop-tags"><span class="stop-tag">Wetlands</span><span class="stop-tag">Cultural</span></div>
          <div class="stop-time">10:30 AM</div>
        </div>
      </div>

      <div class="stop">
        <div class="stop-number">4</div>
        <div class="stop-content">
          <h3>Ancient Cave Paintings</h3>
          <p>The tour's centrepiece: 27 hand prints and 2 feet prints left by Bidjigal and Dharawal artists — up to 6,500 years old. Jessy shares the cultural meaning of the ochre pigments, the technique, and what these markings tell us about the people who made them.</p>
          <div class="stop-tags"><span class="stop-tag">Cultural Heritage</span><span class="stop-tag">Rock Art</span></div>
          <div class="stop-time">10:45 AM</div>
        </div>
      </div>

      <div class="stop">
        <div class="stop-number">5</div>
        <div class="stop-content">
          <h3>Bush Foods Walk — Part I</h3>
          <p>Jessy identifies native edible and medicinal plants along the trail — lomandra for basket weaving, acacia seeds ground for flour, native mint used in healing. You'll taste where appropriate and learn the Dharawal names for key species.</p>
          <div class="stop-tags"><span class="stop-tag">Bush Foods</span><span class="stop-tag">Medicinal Plants</span></div>
          <div class="stop-time">11:00 AM</div>
        </div>
      </div>

      <div class="stop">
        <div class="stop-number">6</div>
        <div class="stop-content">
          <h3>Ochre Rock Mine</h3>
          <p>The source of the pigments used in the cave paintings. Yellow, white and orange sandstone ochre is visible in the rock face here. Jessy explains how ochre was harvested, prepared, and used — for ceremony, body decoration, and art.</p>
          <div class="stop-tags"><span class="stop-tag">Geology</span><span class="stop-tag">Cultural</span></div>
          <div class="stop-time">11:20 AM</div>
        </div>
      </div>

      <div class="stop">
        <div class="stop-number">7</div>
        <div class="stop-content">
          <h3>Bush Regeneration Site</h3>
          <p>See active native planting and weed removal in progress — the slow, patient work of restoring the valley's original plant communities. Jessy explains which species are being reintroduced and why, and how 5% of every tour booking contributes to this work.</p>
          <div class="stop-tags"><span class="stop-tag">Conservation</span></div>
          <div class="stop-time">11:30 AM</div>
        </div>
      </div>

      <div class="stop">
        <div class="stop-number">8</div>
        <div class="stop-content">
          <h3>Grey-headed Flying-fox Colony</h3>
          <p>One of Australia's most significant flying-fox roosts — a threatened species of national importance. The colony at Wolli Creek numbers in the thousands. Jessy shares the ecological role of flying-foxes in pollination and seed dispersal, and their significance in Aboriginal culture.</p>
          <div class="stop-tags"><span class="stop-tag">Wildlife</span></div>
          <div class="stop-time">11:45 AM</div>
        </div>
      </div>

      <div class="stop">
        <div class="stop-number">9</div>
        <div class="stop-content">
          <h3>Bush Foods Walk — Part II &amp; Creek Crossing</h3>
          <p>Continue identifying native species as you walk the creek corridor. Jessy points out trees used for canoe construction, bark for shelter, and plants used in ceremony. Cross the creek and take in the full valley soundscape.</p>
          <div class="stop-tags"><span class="stop-tag">Bush Foods</span><span class="stop-tag">Ecology</span></div>
          <div class="stop-time">12:00 PM</div>
        </div>
      </div>

      <div class="stop">
        <div class="stop-number">10</div>
        <div class="stop-content">
          <h3>End Point — Debrief &amp; Farewells</h3>
          <p>The tour concludes with a group debrief and open Q&amp;A. Jessy shares recommendations for further learning, nearby cultural sites, and how to continue supporting the valley. The group disperses from one of three end points depending on the chosen route.</p>
          <div class="stop-tags"><span class="stop-tag">All abilities</span></div>
          <div class="stop-time">12:45 PM</div>
        </div>
      </div>

    </div>
  </section>

  <!-- PRACTICAL INFO -->
  <section class="practical-section">
    <div class="container">
      <span class="label">Before You Come</span>
      <h2>Practical Information</h2>
      <div class="practical-grid">
        <div class="practical-item">
          <h4>What to Bring</h4>
          <p>Comfortable walking shoes, water (1L minimum), sunscreen and hat, insect repellent, rain jacket if forecast. Light snack optional.</p>
        </div>
        <div class="practical-item">
          <h4>Accessibility</h4>
          <p>The route is on natural bush tracks — uneven ground, some inclines. Not suitable for wheelchairs or prams. Suitable for most fitness levels. Contact Jessy if you have specific accessibility needs.</p>
        </div>
        <div class="practical-item">
          <h4>Cancellations &amp; Weather</h4>
          <p>Tours run in light rain. Cancellations due to severe weather are made by 7am on the day of the tour. Full refund or reschedule offered. See the <a href="policies.html">Bookings &amp; Refunds Policy</a> for full details.</p>
        </div>
      </div>
    </div>
  </section>

  <!-- INLINE CTA -->
  <div class="inline-cta">
    <h2>Ready to Walk Country?</h2>
    <a href="book.html" class="btn btn-primary">Book a Tour</a>
  </div>

  <footer class="site-footer">
    <p>Wolli Creek Valley Cultural Tours · Bidjigal &amp; Dharawal Country · Inner West Sydney</p>
    <p><a href="mailto:jessyhaiyang@gmail.com">jessyhaiyang@gmail.com</a> · <a href="tel:0491701488">0491 701 488</a></p>
  </footer>

  <script src="js/main.js"></script>
</body>
</html>
```

- [ ] **Step 3: Verify in browser**

Open `tours.html`. Check:
- Page hero loads with dark gradient and heading
- Facts bar visible (green strip)
- 10 stops render with numbered circles, descriptions, tags
- Practical info in 3-column grid
- "Tours" nav link is active (highlighted)

- [ ] **Step 4: Commit**

```bash
git add tours.html css/tours.css
git commit -m "feat: build tour detail page with all 10 stops"
```

---

## Task 6: About Jessy Page (about.html)

**Files:**
- Create: `about.html`
- Create: `css/about.css`

- [ ] **Step 1: Create css/about.css**

```css
/* ============================================
   ABOUT.CSS — About Jessy page
   ============================================ */

.about-content { padding: var(--space-lg) 0; background: var(--color-cream); }

.about-layout {
  display: grid;
  grid-template-columns: 1fr 380px;
  gap: var(--space-lg);
  align-items: start;
}
@media (max-width: 900px) {
  .about-layout { grid-template-columns: 1fr; }
  .about-sidebar { order: -1; }
}

.about-body h2 { margin-bottom: var(--space-sm); }
.about-body p { margin-bottom: var(--space-sm); font-size: 15px; line-height: 1.85; }
.about-body p:last-child { margin-bottom: 0; }

.about-photo {
  border-radius: 8px;
  overflow: hidden;
  background: linear-gradient(135deg, var(--color-forest-mid), var(--color-sage));
  height: 400px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: rgba(212,233,194,0.5);
  font-family: var(--font-sans);
  font-size: 12px;
  letter-spacing: 1px;
  position: sticky;
  top: calc(var(--nav-height) + 20px);
}
.about-photo img { width: 100%; height: 100%; object-fit: cover; }

/* Credentials */
.credentials {
  background: var(--color-forest);
  padding: var(--space-lg) 0;
}
.credentials h2 { color: var(--color-sage-light); margin-bottom: var(--space-md); }
.cred-list {
  list-style: none;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: var(--space-sm);
}
@media (max-width: 600px) { .cred-list { grid-template-columns: 1fr; } }
.cred-list li {
  background: rgba(82,183,136,0.1);
  border: 1px solid rgba(82,183,136,0.2);
  border-radius: 6px;
  padding: 16px;
  font-family: var(--font-sans);
  font-size: 13px;
  color: rgba(212,233,194,0.85);
  line-height: 1.5;
}
.cred-list li strong { display: block; color: var(--color-sage-light); margin-bottom: 2px; }

/* Acknowledgement */
.acknowledgement {
  background: var(--color-forest-dark);
  padding: var(--space-lg) var(--space-md);
  text-align: center;
}
.acknowledgement blockquote {
  max-width: 680px;
  margin: 0 auto;
  font-size: 1.1rem;
  line-height: 1.8;
  color: rgba(212,233,194,0.85);
  border-left: 3px solid var(--color-sage);
  padding-left: var(--space-md);
  text-align: left;
}
.acknowledgement cite { display: block; margin-top: 12px; font-style: normal; font-size: 0.85em; color: rgba(212,233,194,0.45); }

/* Gallery */
.gallery-section { background: var(--color-cream); padding: var(--space-lg) 0; }
.gallery-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 12px;
  margin-top: var(--space-md);
}
@media (max-width: 600px) { .gallery-grid { grid-template-columns: 1fr 1fr; } }
.gallery-img {
  border-radius: 6px;
  overflow: hidden;
  height: 180px;
  background: linear-gradient(135deg, var(--color-forest), var(--color-forest-mid));
}
.gallery-img img { width: 100%; height: 100%; object-fit: cover; }
```

- [ ] **Step 2: Create about.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Meet Jessy Haiyang — descendant of the Pitjara and Iningai peoples, guide, botanist, and founder of Wolli Creek Valley Cultural Tours.">
  <title>About Jessy — Wolli Creek Cultural Tours</title>
  <link rel="stylesheet" href="css/base.css">
  <link rel="stylesheet" href="css/components.css">
  <link rel="stylesheet" href="css/about.css">
</head>
<body>

  <nav class="site-nav">
    <a href="index.html" class="nav-logo">Wolli Creek <span>Cultural Tours</span></a>
    <ul class="nav-links">
      <li><a href="tours.html">Tours</a></li>
      <li><a href="about.html" class="active">About</a></li>
      <li><a href="policies.html">Policies</a></li>
      <li><a href="book.html">Contact</a></li>
    </ul>
    <a href="book.html" class="nav-cta">Book Now</a>
    <button class="nav-toggle" aria-label="Open menu" aria-expanded="false">
      <span></span><span></span><span></span>
    </button>
  </nav>

  <header class="page-hero">
    <span class="label">Your Guide</span>
    <h1><strong>Jessy Haiyang</strong></h1>
    <p>Descendant of the Pitjara and Iningai peoples of central Queensland · Guide &amp; Founder</p>
  </header>

  <!-- MAIN CONTENT -->
  <section class="about-content">
    <div class="container">
      <div class="about-layout">

        <div class="about-body">
          <span class="label">Her Story</span>
          <h2>Walking Country</h2>
          <p>Jessy Haiyang is a descendant of the Pitjara and Iningai peoples of central Queensland. She has lived in Marrickville — on Bidjigal and Dharawal Country — for six years, and in that time has developed a deep, working knowledge of the Wolli Creek Valley: its ecology, its cultural history, and its continued significance to the Eora nations.</p>
          <p>Her connection to the valley began through study. Jessy completed her Diploma of Conservation and Land Management with the Wolli Creek Valley as her primary field of research. She walked every metre of the trail, identified the native species, mapped the cultural sites, and built relationships with the communities who know this place best.</p>
          <p>But knowledge became something more personal. Jessy began to see the valley not just as a study subject but as a living place that deserved to be known more widely — a place where Sydney could encounter the depth and richness of Eora culture not in a gallery, but on Country itself, under open sky, beside running water, in the shadow of 6,500-year-old cave paintings.</p>
          <p>In 2025, Jessy founded Wolli Creek Valley Cultural Guided Tours to make that encounter possible. Every tour is shaped by her belief that cultural knowledge belongs in place — that stories mean more when you can see, touch, smell, and taste the landscape they come from.</p>
          <p>She is committed to sustainability, to truth-telling, and to ensuring that 5% of every booking goes directly back into bush regeneration in the valley she loves.</p>
        </div>

        <div class="about-sidebar">
          <div class="about-photo">
            <!-- Replace with: <img src="images/jessy-portrait.jpg" alt="Jessy Haiyang in the Wolli Creek Valley"> -->
            Photo coming soon
          </div>
        </div>

      </div>
    </div>
  </section>

  <!-- CREDENTIALS -->
  <section class="credentials">
    <div class="container">
      <span class="label">Qualifications</span>
      <h2>Credentials</h2>
      <ul class="cred-list">
        <li><strong>Diploma of Conservation and Land Management</strong>Field research based on the Wolli Creek Valley ecosystem</li>
        <li><strong>First Aid Certificate</strong>Current certification — carried on every tour</li>
        <li><strong>Tour Guide Certification</strong>Licensed cultural and eco-tourism guide</li>
        <li><strong>Working With Children Check</strong>Current NSW Working With Children Check</li>
        <li><strong>Public Liability Insurance</strong>Fully insured for guided tours on public land</li>
        <li><strong>Safe Work Plan</strong>Comprehensive WHS procedures for all tour operations</li>
      </ul>
    </div>
  </section>

  <!-- ACKNOWLEDGEMENT OF COUNTRY -->
  <section class="acknowledgement">
    <span class="label" style="color:var(--color-sage);display:block;margin-bottom:24px;">Acknowledgement of Country</span>
    <blockquote>
      "I acknowledge the Bidjigal and Dharawal peoples as the Traditional Custodians of the land on which we walk. I pay my respects to Elders past, present and emerging, and to all Aboriginal and Torres Strait Islander peoples. The stories, knowledge, and cultural practices I share on this tour are offered in the spirit of connection, respect, and truth-telling."
      <cite>— Jessy Haiyang</cite>
    </blockquote>
  </section>

  <!-- GALLERY -->
  <section class="gallery-section">
    <div class="container">
      <span class="label">The Valley</span>
      <h2>Wolli Creek in Pictures</h2>
      <div class="gallery-grid">
        <div class="gallery-img">
          <img src="images/Bush regen site 1.jpg" alt="Bush regeneration site in Wolli Creek Valley" onerror="this.style.display='none'">
        </div>
        <div class="gallery-img">
          <img src="images/Bush regen site 2.jpg" alt="Native plants being restored in the valley" onerror="this.style.display='none'">
        </div>
        <div class="gallery-img">
          <img src="images/Bush regen site 3.jpg" alt="Creek corridor with native vegetation" onerror="this.style.display='none'">
        </div>
      </div>
    </div>
  </section>

  <div style="background:var(--color-forest);padding:var(--space-lg) var(--space-md);text-align:center;">
    <a href="book.html" class="btn btn-primary">Book a Tour with Jessy</a>
  </div>

  <footer class="site-footer">
    <p>Wolli Creek Valley Cultural Tours · Bidjigal &amp; Dharawal Country · Inner West Sydney</p>
    <p><a href="mailto:jessyhaiyang@gmail.com">jessyhaiyang@gmail.com</a> · <a href="tel:0491701488">0491 701 488</a></p>
  </footer>

  <script src="js/main.js"></script>
</body>
</html>
```

- [ ] **Step 3: Copy photos into images directory**

The project root has these image files — copy them into `images/`:

```bash
cp "Bush regen site 1.jpg" images/
cp "Bush regen site 2.jpg" images/
cp "Bush regen site 3.jpg" images/
cp "1000026691.png" images/
cp "1000026692.png" images/
cp "1000026693.png" images/
```

- [ ] **Step 4: Verify in browser**

Open `about.html`. Check:
- Page hero loads, Jessy's name prominent
- Two-column layout: story text left, photo placeholder right (sticky on scroll)
- Credentials grid in dark green section
- Acknowledgement of Country centred with quote style
- Gallery: bush regen photos display if images are in place

- [ ] **Step 5: Commit**

```bash
git add about.html css/about.css images/
git commit -m "feat: build About Jessy page with story, credentials, acknowledgement, gallery"
```

---

## Task 7: Bookings & Contact Page (book.html)

**Files:**
- Create: `book.html`
- Create: `css/book.css`

- [ ] **Step 1: Create css/book.css**

```css
/* ============================================
   BOOK.CSS — Bookings & contact page
   ============================================ */

.book-layout {
  display: grid;
  grid-template-columns: 1fr 400px;
  gap: var(--space-lg);
  padding: var(--space-lg) 0;
  align-items: start;
}
@media (max-width: 900px) {
  .book-layout { grid-template-columns: 1fr; }
}

/* How to book steps */
.steps { list-style: none; margin-bottom: var(--space-md); }
.steps li {
  display: flex;
  gap: var(--space-sm);
  align-items: flex-start;
  padding: var(--space-sm) 0;
  border-bottom: 1px solid rgba(82,183,136,0.12);
}
.steps li:last-child { border-bottom: none; }
.step-num {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background: var(--color-sage);
  color: var(--color-forest-dark);
  font-family: var(--font-sans);
  font-size: 13px;
  font-weight: 700;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}
.step-text h4 { font-size: 0.9rem; margin-bottom: 4px; font-family: var(--font-sans); font-weight: 600; }
.step-text p { font-size: 13px; }

/* Contact form */
.contact-form { background: #fff; border-radius: 8px; padding: var(--space-md); box-shadow: 0 2px 16px rgba(27,58,45,0.08); }
.contact-form h3 { margin-bottom: var(--space-sm); font-size: 1.1rem; }

.form-group { margin-bottom: var(--space-sm); }
.form-group label {
  display: block;
  font-family: var(--font-sans);
  font-size: 11px;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: var(--color-text-muted);
  margin-bottom: 6px;
  font-weight: 600;
}
.form-group input,
.form-group select,
.form-group textarea {
  width: 100%;
  padding: 10px 12px;
  border: 1px solid rgba(82,183,136,0.3);
  border-radius: 4px;
  font-family: var(--font-sans);
  font-size: 14px;
  color: var(--color-text);
  background: var(--color-cream);
  transition: border-color 0.15s;
  appearance: none;
}
.form-group input:focus,
.form-group select:focus,
.form-group textarea:focus {
  outline: none;
  border-color: var(--color-sage);
}
.form-group textarea { resize: vertical; min-height: 100px; }
.form-row { display: grid; grid-template-columns: 1fr 1fr; gap: var(--space-sm); }
@media (max-width: 480px) { .form-row { grid-template-columns: 1fr; } }

/* Direct contact panel */
.direct-contact {
  background: var(--color-forest);
  border-radius: 8px;
  padding: var(--space-md);
  margin-top: var(--space-md);
}
.direct-contact h3 { color: var(--color-sage-light); margin-bottom: var(--space-sm); font-size: 1rem; }
.contact-method {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 10px 0;
  border-bottom: 1px solid rgba(82,183,136,0.15);
}
.contact-method:last-child { border-bottom: none; }
.contact-method-label { font-family: var(--font-sans); font-size: 10px; letter-spacing: 1px; text-transform: uppercase; color: rgba(212,233,194,0.5); display: block; margin-bottom: 2px; }
.contact-method a { color: var(--color-sage-light); font-size: 14px; text-decoration: none; }
.contact-method a:hover { color: var(--color-sage); }

/* Pricing */
.pricing-section { background: var(--color-cream); padding: var(--space-lg) 0; border-top: 1px solid rgba(82,183,136,0.15); }
.price-cards {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: var(--space-sm);
  margin-top: var(--space-md);
}
@media (max-width: 700px) { .price-cards { grid-template-columns: 1fr; } }
.price-card {
  background: #fff;
  border-radius: 8px;
  padding: var(--space-md);
  text-align: center;
  box-shadow: 0 2px 12px rgba(27,58,45,0.06);
}
.price-card h3 { font-size: 0.9rem; font-family: var(--font-sans); text-transform: uppercase; letter-spacing: 1px; font-weight: 700; margin-bottom: 12px; color: var(--color-text-muted); }
.price-card .price { font-size: 2rem; font-family: var(--font-serif); color: var(--color-forest); margin-bottom: 8px; }
.price-card .price-note { font-size: 12px; color: var(--color-text-muted); }
```

- [ ] **Step 2: Create book.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Book a Wolli Creek Valley Cultural Guided Tour with Jessy Haiyang. Contact Jessy directly to arrange your tour date.">
  <title>Book a Tour — Wolli Creek Cultural Tours</title>
  <link rel="stylesheet" href="css/base.css">
  <link rel="stylesheet" href="css/components.css">
  <link rel="stylesheet" href="css/book.css">
</head>
<body>

  <nav class="site-nav">
    <a href="index.html" class="nav-logo">Wolli Creek <span>Cultural Tours</span></a>
    <ul class="nav-links">
      <li><a href="tours.html">Tours</a></li>
      <li><a href="about.html">About</a></li>
      <li><a href="policies.html">Policies</a></li>
      <li><a href="book.html" class="active">Contact</a></li>
    </ul>
    <a href="book.html" class="nav-cta">Book Now</a>
    <button class="nav-toggle" aria-label="Open menu" aria-expanded="false">
      <span></span><span></span><span></span>
    </button>
  </nav>

  <header class="page-hero">
    <span class="label">Book a Tour</span>
    <h1><strong>Walk Country</strong><br>with Jessy</h1>
    <p>Small groups · 2.75 hours · Tempe Station Carpark · All abilities welcome</p>
  </header>

  <!-- PRICING -->
  <section class="pricing-section">
    <div class="container">
      <span class="label">Pricing</span>
      <h2>Tour Rates</h2>
      <div class="price-cards">
        <div class="price-card">
          <h3>Adult</h3>
          <div class="price">TBC</div>
          <p class="price-note">Per person · Includes all stops</p>
        </div>
        <div class="price-card">
          <h3>Concession</h3>
          <div class="price">TBC</div>
          <p class="price-note">Pensioner / student / health care card</p>
        </div>
        <div class="price-card">
          <h3>Private Group</h3>
          <div class="price">Contact</div>
          <p class="price-note">Groups of 8+ · Schools &amp; organisations welcome</p>
        </div>
      </div>
      <p style="margin-top:16px;font-size:13px;">Pricing will be confirmed closer to the launch of public tours. <a href="mailto:jessyhaiyang@gmail.com" style="color:var(--color-forest-mid);text-decoration:underline;">Contact Jessy</a> to register your interest or enquire about private bookings.</p>
    </div>
  </section>

  <!-- BOOKING FORM + HOW TO BOOK -->
  <div style="background:var(--color-cream);padding-bottom:var(--space-lg);">
    <div class="container">
      <div class="book-layout">

        <!-- Left: How to book -->
        <div>
          <span class="label">How It Works</span>
          <h2>Booking a Tour</h2>
          <p style="margin:8px 0 var(--space-md);">Tours are booked directly with Jessy — no third-party platform, no booking fees.</p>

          <ol class="steps">
            <li>
              <div class="step-num">1</div>
              <div class="step-text">
                <h4>Choose a Date</h4>
                <p>Tours run on selected weekends. Contact Jessy to find out available dates, or suggest a date that suits your group.</p>
              </div>
            </li>
            <li>
              <div class="step-num">2</div>
              <div class="step-text">
                <h4>Send an Enquiry</h4>
                <p>Use the form on this page, email jessyhaiyang@gmail.com, or call 0491 701 488. Include your preferred date, group size, and any accessibility requirements.</p>
              </div>
            </li>
            <li>
              <div class="step-num">3</div>
              <div class="step-text">
                <h4>Receive Confirmation</h4>
                <p>Jessy will respond within 48 hours to confirm your booking and provide meeting point details, payment instructions, and what to bring.</p>
              </div>
            </li>
            <li>
              <div class="step-num">4</div>
              <div class="step-text">
                <h4>Walk Country</h4>
                <p>Meet Jessy at Tempe Station Carpark at your confirmed time. Tours begin promptly — please arrive 5 minutes early.</p>
              </div>
            </li>
          </ol>

          <div class="direct-contact">
            <h3>Contact Jessy Directly</h3>
            <div class="contact-method">
              <div>
                <span class="contact-method-label">Email</span>
                <a href="mailto:jessyhaiyang@gmail.com">jessyhaiyang@gmail.com</a>
              </div>
            </div>
            <div class="contact-method">
              <div>
                <span class="contact-method-label">Phone</span>
                <a href="tel:0491701488">0491 701 488</a>
              </div>
            </div>
            <div class="contact-method">
              <div>
                <span class="contact-method-label">Meeting Point</span>
                <a href="https://maps.apple.com/?q=Tempe+Station" target="_blank" rel="noopener">Tempe Station Carpark, Tempe NSW 2044</a>
              </div>
            </div>
          </div>
        </div>

        <!-- Right: Contact form -->
        <div>
          <div class="contact-form">
            <h3>Send an Enquiry</h3>
            <form action="mailto:jessyhaiyang@gmail.com" method="post" enctype="text/plain">
              <div class="form-row">
                <div class="form-group">
                  <label for="name">Your Name</label>
                  <input type="text" id="name" name="name" placeholder="First &amp; last name" required>
                </div>
                <div class="form-group">
                  <label for="phone">Phone</label>
                  <input type="tel" id="phone" name="phone" placeholder="04XX XXX XXX">
                </div>
              </div>
              <div class="form-group">
                <label for="email">Email Address</label>
                <input type="email" id="email" name="email" placeholder="you@email.com" required>
              </div>
              <div class="form-row">
                <div class="form-group">
                  <label for="date">Preferred Date</label>
                  <input type="date" id="date" name="date">
                </div>
                <div class="form-group">
                  <label for="group">Group Size</label>
                  <select id="group" name="group">
                    <option value="">Select...</option>
                    <option value="1">1 person</option>
                    <option value="2">2 people</option>
                    <option value="3-5">3–5 people</option>
                    <option value="6-10">6–10 people</option>
                    <option value="11+">11+ people</option>
                  </select>
                </div>
              </div>
              <div class="form-group">
                <label for="message">Message</label>
                <textarea id="message" name="message" placeholder="Any questions, accessibility requirements, or special requests..."></textarea>
              </div>
              <button type="submit" class="btn btn-primary" style="width:100%;">Send Enquiry</button>
            </form>
          </div>
        </div>

      </div>
    </div>
  </div>

  <footer class="site-footer">
    <p>Wolli Creek Valley Cultural Tours · Bidjigal &amp; Dharawal Country · Inner West Sydney</p>
    <p><a href="mailto:jessyhaiyang@gmail.com">jessyhaiyang@gmail.com</a> · <a href="tel:0491701488">0491 701 488</a></p>
  </footer>

  <script src="js/main.js"></script>
</body>
</html>
```

- [ ] **Step 3: Verify in browser**

Open `book.html`. Check:
- Pricing cards render (3 columns)
- How-to-book steps numbered correctly
- Contact form fields styled consistently
- Direct contact panel (dark green) with email/phone
- Mobile: columns stack correctly

- [ ] **Step 4: Commit**

```bash
git add book.html css/book.css
git commit -m "feat: build bookings and contact page with enquiry form and pricing"
```

---

## Task 8: Policies Page (policies.html)

**Files:**
- Create: `policies.html`
- Create: `css/policies.css`

- [ ] **Step 1: Create css/policies.css**

```css
/* ============================================
   POLICIES.CSS — Policies page
   ============================================ */

.policies-layout {
  display: grid;
  grid-template-columns: 220px 1fr;
  gap: var(--space-lg);
  padding: var(--space-lg) 0;
  align-items: start;
}
@media (max-width: 768px) {
  .policies-layout { grid-template-columns: 1fr; }
  .policies-sidebar { display: none; }
}

/* Sidebar nav */
.policies-sidebar {
  position: sticky;
  top: calc(var(--nav-height) + 20px);
}
.policies-sidebar h4 {
  font-family: var(--font-sans);
  font-size: 10px;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: var(--color-text-muted);
  margin-bottom: 12px;
}
.sidebar-nav { list-style: none; }
.sidebar-nav li { margin-bottom: 4px; }
.sidebar-nav a {
  font-family: var(--font-sans);
  font-size: 13px;
  color: var(--color-text-muted);
  text-decoration: none;
  padding: 6px 10px;
  border-radius: 4px;
  display: block;
  transition: background 0.15s, color 0.15s;
  border-left: 2px solid transparent;
}
.sidebar-nav a:hover {
  background: rgba(82,183,136,0.1);
  color: var(--color-text);
  border-left-color: var(--color-sage);
}

/* Policy document */
.policy-doc { margin-bottom: var(--space-xl); padding-bottom: var(--space-lg); border-bottom: 1px solid rgba(82,183,136,0.15); }
.policy-doc:last-child { border-bottom: none; }
.policy-doc h2 { font-size: 1.5rem; margin-bottom: 8px; }
.policy-doc h3 { font-size: 1rem; margin: var(--space-sm) 0 8px; font-family: var(--font-sans); font-weight: 700; text-transform: uppercase; letter-spacing: 1px; font-size: 11px; color: var(--color-text-muted); }
.policy-doc p { margin-bottom: 12px; font-size: 14px; line-height: 1.8; }
.policy-doc ul, .policy-doc ol { margin: 8px 0 12px 20px; }
.policy-doc li { font-family: var(--font-sans); font-size: 14px; line-height: 1.75; color: var(--color-text-muted); margin-bottom: 4px; }
```

- [ ] **Step 2: Create policies.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Tour policies for Wolli Creek Valley Cultural Tours — bookings, refunds, safety procedures, and workplace health and safety.">
  <title>Policies — Wolli Creek Cultural Tours</title>
  <link rel="stylesheet" href="css/base.css">
  <link rel="stylesheet" href="css/components.css">
  <link rel="stylesheet" href="css/policies.css">
</head>
<body>

  <nav class="site-nav">
    <a href="index.html" class="nav-logo">Wolli Creek <span>Cultural Tours</span></a>
    <ul class="nav-links">
      <li><a href="tours.html">Tours</a></li>
      <li><a href="about.html">About</a></li>
      <li><a href="policies.html" class="active">Policies</a></li>
      <li><a href="book.html">Contact</a></li>
    </ul>
    <a href="book.html" class="nav-cta">Book Now</a>
    <button class="nav-toggle" aria-label="Open menu" aria-expanded="false">
      <span></span><span></span><span></span>
    </button>
  </nav>

  <header class="page-hero">
    <span class="label">Information</span>
    <h1><strong>Tour Policies</strong></h1>
    <p>Bookings, refunds, safety procedures, and workplace health &amp; safety</p>
  </header>

  <div style="background:var(--color-cream);padding-bottom:var(--space-lg);">
    <div class="container">
      <div class="policies-layout">

        <!-- Sidebar -->
        <aside class="policies-sidebar">
          <h4>On This Page</h4>
          <ul class="sidebar-nav">
            <li><a href="#bookings-refunds">Bookings &amp; Refunds</a></li>
            <li><a href="#safety">Safe Work Plan</a></li>
            <li><a href="#property">Property &amp; Land</a></li>
          </ul>
        </aside>

        <!-- Policies content -->
        <div>

          <article class="policy-doc" id="bookings-refunds">
            <span class="label">Policy 1</span>
            <h2>Bookings &amp; Refunds Policy</h2>
            <p><em>Wolli Creek Valley Cultural Guided Tours — 2025</em></p>

            <h3>Bookings</h3>
            <p>All tour bookings are made directly with Jessy Haiyang via email or phone. A booking is confirmed once Jessy has acknowledged your enquiry and confirmed the tour date, group size, and meeting details in writing.</p>
            <p>Payment is due at the time of booking confirmation. Jessy will provide bank transfer details upon confirmation. Tours are not held without payment.</p>

            <h3>Cancellations by the Client</h3>
            <ul>
              <li><strong>More than 7 days before the tour:</strong> Full refund provided.</li>
              <li><strong>3–7 days before the tour:</strong> 50% refund, or the option to reschedule to another available date at no cost.</li>
              <li><strong>Less than 3 days before the tour:</strong> No refund. Reschedule option available at Jessy's discretion.</li>
              <li><strong>No-show on the day:</strong> No refund.</li>
            </ul>

            <h3>Cancellations by Wolli Creek Cultural Tours</h3>
            <p>Jessy reserves the right to cancel or reschedule a tour in the event of severe weather, health or safety concerns, or other circumstances outside her control. In these cases, all clients will receive a full refund or be offered a reschedule at no cost.</p>
            <p>Cancellations due to weather will be communicated by 7:00 AM on the day of the tour via the contact details provided at booking.</p>

            <h3>Group Bookings</h3>
            <p>For private group bookings of 8 or more participants, a 25% deposit is required at the time of booking. The balance is due 7 days before the tour. Cancellation terms above apply to the total booking amount.</p>

            <h3>Enquiries</h3>
            <p>For any questions about bookings or refunds, contact Jessy directly:<br>
            <a href="mailto:jessyhaiyang@gmail.com">jessyhaiyang@gmail.com</a> · <a href="tel:0491701488">0491 701 488</a></p>
          </article>

          <article class="policy-doc" id="safety">
            <span class="label">Policy 2</span>
            <h2>Safe Work Plan</h2>
            <p><em>Workplace Health &amp; Safety Policy and Procedures — Wolli Creek Valley Cultural Guided Tours</em></p>

            <h3>Purpose</h3>
            <p>This Safe Work Plan outlines the health and safety procedures for all guided tour operations conducted by Wolli Creek Valley Cultural Guided Tours. The safety of all participants and the guide is the highest priority on every tour.</p>

            <h3>Before the Tour</h3>
            <ul>
              <li>Weather forecast checked. Tours cancelled in severe weather (thunderstorms, extreme heat above 40°C, or total fire ban days).</li>
              <li>First aid kit carried by guide on all tours.</li>
              <li>Guide to carry mobile phone with full charge and emergency contacts saved.</li>
              <li>Participant numbers confirmed — maximum 15 persons per tour.</li>
              <li>All participants briefed on safety procedures at the start of the tour.</li>
            </ul>

            <h3>On-Tour Safety Briefing</h3>
            <p>At the start of every tour, the guide will:</p>
            <ul>
              <li>Identify themselves and confirm the tour route and duration.</li>
              <li>Ask participants to disclose any medical conditions, allergies, or mobility requirements.</li>
              <li>Point out any known hazards on the route (uneven ground, creek crossings, slippery surfaces).</li>
              <li>Instruct participants to stay together and not wander from the group.</li>
              <li>Provide emergency contact details and the location of the nearest first aid kit.</li>
            </ul>

            <h3>Hazards and Controls</h3>
            <ul>
              <li><strong>Uneven terrain:</strong> Participants advised to wear appropriate footwear. Slow pace on difficult sections.</li>
              <li><strong>Sun and heat:</strong> Participants advised to bring hat, sunscreen, and minimum 1L water. Tours not conducted in extreme heat.</li>
              <li><strong>Wildlife:</strong> Participants instructed not to approach, feed, or touch wildlife. Guide maintains safe distance from flying-fox colony.</li>
              <li><strong>Bush foods tasting:</strong> Only known safe species offered. Participants with food allergies asked to declare at the start of the tour.</li>
              <li><strong>Creek crossings:</strong> Guide assesses creek level before crossing. High water: crossing bypassed.</li>
              <li><strong>Medical emergency:</strong> Guide trained in First Aid. Call 000 for emergencies. Nearest hospital: St George Hospital, Kogarah.</li>
            </ul>

            <h3>Incident Reporting</h3>
            <p>Any incident, near-miss, or injury occurring during a tour must be documented by the guide within 24 hours. Serious incidents must be reported to Safe Work NSW as required by law.</p>
          </article>

          <article class="policy-doc" id="property">
            <span class="label">Policy 3</span>
            <h2>Property &amp; Land Acknowledgement</h2>
            <p><em>Wolli Creek Valley and Turrella Reserve — Property Information</em></p>

            <h3>Crown Lands NSW</h3>
            <p>Tours operate within the Wolli Creek Valley Regional Park and Turrella Reserve, which are managed by Crown Lands NSW and local councils. All tour operations comply with relevant Crown Lands regulations and any conditions attached to the use of these lands for guided tours.</p>

            <h3>Cultural Sites</h3>
            <p>Cave paintings and other cultural heritage sites visited on the tour are protected under the NSW Heritage Act 1977 and the Aboriginal Land Rights Act 1983. Participants must not touch, mark, or disturb any cultural heritage site. Photography is permitted from a respectful distance.</p>

            <h3>Leave No Trace</h3>
            <p>All participants are expected to respect the environment. No litter, no disturbance of native plants or wildlife, and no collection of natural materials (rocks, plants, soil) from the reserve.</p>

            <h3>Bush Regeneration</h3>
            <p>Five percent of all tour revenue is directed to bush regeneration activities in the Wolli Creek Valley, including native planting and weed removal. This is a direct commitment by Wolli Creek Valley Cultural Tours to the ongoing health of the Country on which tours operate.</p>
          </article>

        </div>
      </div>
    </div>
  </div>

  <footer class="site-footer">
    <p>Wolli Creek Valley Cultural Tours · Bidjigal &amp; Dharawal Country · Inner West Sydney</p>
    <p><a href="mailto:jessyhaiyang@gmail.com">jessyhaiyang@gmail.com</a> · <a href="tel:0491701488">0491 701 488</a></p>
  </footer>

  <script src="js/main.js"></script>
</body>
</html>
```

- [ ] **Step 3: Verify in browser**

Open `policies.html`. Check:
- Sidebar nav visible on desktop, sticky on scroll
- Three policy sections with clear headings and sub-headings
- Lists render with proper indent
- Sidebar links (#bookings-refunds, #safety, #property) jump to correct sections

- [ ] **Step 4: Commit**

```bash
git add policies.html css/policies.css
git commit -m "feat: build policies page with bookings, safety, and land acknowledgement"
```

---

## Task 9: Final Polish — Responsive Check + Meta Tags + Favicon

**Files:**
- Modify: all HTML files (meta tags)

- [ ] **Step 1: Open each page in Chrome DevTools mobile emulation**

For each of the 5 pages, open DevTools (F12) → Toggle Device Toolbar → select "iPhone SE" (375px width). Check:
- Nav: hamburger appears, links hidden, opens on tap
- Homepage cards: single column
- About layout: stacked, photo full width
- Book layout: form below steps
- Stops: readable, circles don't overlap text

Fix any layout issues in the relevant CSS file.

- [ ] **Step 2: Add Open Graph meta tags to index.html**

In the `<head>` of `index.html`, after the existing `<meta name="description">` tag, add:

```html
  <meta property="og:title" content="Wolli Creek Cultural Tours — Walk the Valley">
  <meta property="og:description" content="Indigenous-led guided tours of the Wolli Creek Valley on Bidjigal and Dharawal Country, Inner West Sydney.">
  <meta property="og:type" content="website">
```

- [ ] **Step 3: Verify all internal links work**

Open `index.html` and click every link:
- Nav: Tours → tours.html, About → about.html, Policies → policies.html, Contact → book.html, Book Now → book.html
- Homepage: "See All 10 Stops →" → tours.html, "Read Jessy's Story →" → about.html, "Book Now" → book.html
- All pages: Footer email/phone links open mail client/phone dialler

- [ ] **Step 4: Commit**

```bash
git add index.html tours.html about.html book.html policies.html css/
git commit -m "feat: final polish — responsive fixes, OG meta tags, link audit"
```

---

## Self-Review Notes

**Spec coverage:**
- ✅ Homepage (hero, stats, highlights grid, about strip, CTA, footer)
- ✅ Tour detail (all 10 stops, facts bar, practical info, inline CTA)
- ✅ About Jessy (story, credentials, acknowledgement, gallery)
- ✅ Bookings & contact (pricing, how-to-book, contact form, direct contact)
- ✅ Policies (bookings & refunds, safe work plan, property acknowledgement)
- ✅ Shared nav/footer, mobile responsive, CSS design system
- ⚠️ **Pricing on book.html is "TBC"** — Jessy needs to confirm per-person rates before launch. Placeholder text directs visitors to contact Jessy directly, which is correct for now.
- ⚠️ **Stops 9 & 10 confirmed from context** — content written from available tour documents; verify final stop names/details with Jessy's itinerary before launch.

**Type/naming consistency:** All CSS class names used in HTML match definitions in CSS files. All internal links use consistent filenames (index.html, tours.html, about.html, book.html, policies.html).
