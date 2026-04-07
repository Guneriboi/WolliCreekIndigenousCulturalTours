# Wolli Creek Cultural Tours — Website Design Spec
**Date:** 2026-04-07  
**Project:** Wolli Creek Valley Cultural Guided Tours  
**Guide & Founder:** Jessy Haiyang

---

## Overview

A public-facing website for Wolli Creek Valley Cultural Guided Tours — a small Indigenous-led tour business operating on Bidjigal and Dharawal Country in the Inner West of Sydney. The site must convey cultural richness and authenticity, drive bookings, and serve as the primary online presence for the business.

**Design reference:** knowtogo.pt aesthetic — clean, editorial, photo-rich, card-driven.

---

## Approach Selected: Hybrid — Story Hero + Card Grid (Option C)

A cinematic full-width hero section creates an emotional hook, followed by a card grid of tour highlights. Inner pages are content-focused. This balances cultural storytelling with practical booking information, and works even with a limited initial photo library.

---

## Visual Design System

| Token | Value |
|---|---|
| Deep Forest (primary bg) | `#1B3A2D` |
| Dark Forest (hero bg) | `#0D2218` |
| Mid Forest (accent bg) | `#2D6A4F` |
| Sage Green (CTA/accent) | `#52B788` |
| Cream (page bg) | `#f5f0e8` |
| Light Sage (text on dark) | `#D4E9C2` |
| Ochre (geology accent) | `#8B6914` |

**Typography:** Georgia (serif) for headings and body — editorial feel. Sans-serif (system) for labels, captions, and UI elements. Uppercase letter-spaced labels throughout.

**Tone:** Respectful, culturally grounded, quietly proud. Not touristy. Not corporate.

---

## Site Structure

```
/                   Homepage
/tours              Tour detail (the walk)
/about              About Jessy
/book               Bookings & contact
/policies           Policies (refunds, safety, workplace)
```

---

## Page Designs

### 1. Homepage (`/`)

**Nav (sticky):**  
Dark forest background. Logo: "Wolli Creek · Cultural Tours" in sage. Nav links: Tours, About, Journal, Policies, Contact. "Book Now" CTA button (sage, always visible).

**Hero section:**  
- Full-height (88vh), dark gradient background (deep forest → mid forest)
- Badge: "Bidjigal & Dharawal Country · Inner West Sydney"
- Headline: **"Walk the Valley. Hear the Stories of Country."**
- Subline: "Wolli Creek Valley · Est. 2025"
- Pull quote from Jessy: *"I would like to create the opportunity for Sydney to showcase the rich natural history of the Eora nations people on Country — sharing bush foods, ancient cave paintings, and the living knowledge of our ancestors."*
- Two buttons: "Book a Tour" (primary) + "Explore the Walk →" (ghost)
- Scroll indicator

**Stats band:**  
Mid-forest background. Five stats: 2.75 hrs guided walk · 10 landmark stops · 6,500 yrs ancient cave paintings · 100+ native bird species · 3 start/end points.

**Tour Highlights grid:**  
Cream background. Section label "Tour Highlights", heading "What You'll Experience". 3-column card grid (2 rows = 6 featured stops), each card has:  
- Colour-coded image area with category tag
- Coloured accent line (matches ecology type)
- Stop name, short description, time/stop number

Six featured highlights:
1. Ancient Cave Paintings (Cultural Heritage · Stop 4)
2. Bush Foods & Medicinal Plants (Bush Foods · Stops 3 & 6)
3. Grey-headed Flying-fox Colony (Wildlife · Stop 8)
4. Ochre Rock Mine (Geology · Stop 7)
5. Tidal Salt Water Marshlands (Wetlands · Stop 3)
6. Bush Regeneration Sites (Conservation · throughout)

**About strip:**  
Dark forest background, two-column layout. Left: Jessy's bio summary + "Read Jessy's Story →" button. Right: Jessy's photo placeholder.

**CTA section:**  
Sage gradient. "Ready to Walk Country?" heading. Practical info line (departure point, group size, duration, accessibility). Three buttons: Book Now · Enquire via Email · Call Jessy.

**Footer:**  
Darkest forest. Business name, Country acknowledgement, email, phone.

---

### 2. Tour Detail (`/tours`)

**Purpose:** Full walk breakdown — what happens, when, where, and why it matters.

**Sections:**
- Hero: landscape photo of the valley, tour name overlay
- Quick facts bar: duration, distance, difficulty, group size, price, meeting point
- The Walk: narrative description of the tour experience (300–400 words, Jessy's voice)
- All 10 stops: timeline/list layout — stop number, name, category tag, description, approximate time
- Practical info: what to bring, accessibility notes, weather/cancellation info
- Inline CTA: "Ready to Book?" with button

**Stop list (all 10):**
1. Tempe Station Carpark — meeting & Welcome to Country
2. Wolli Creek entrance — Valley overview & ecology intro
3. Tidal Salt Water Marshlands — tribal meeting point, shellfish beds
4. Ancient Cave Paintings — 27 handprints, 2 feet prints, 6,500 years old
5. Ochre Rock Mine — sandstone pigment source
6. Bush Foods & Medicine walk — edible plants, tools, healing
7. Flying-fox Colony roost — endangered species, national importance
8. Bush Regeneration site — active native planting
9. Creek crossing / water feature — creek ecology (confirm name from itinerary doc)
10. End point — debrief, Q&A, farewells (confirm location from itinerary doc)

---

### 3. About Jessy (`/about`)

**Purpose:** Build trust and cultural connection. This page is the human heart of the site.

**Sections:**
- Full-width portrait photo of Jessy (or landscape with her in it)
- Headline: "Jessy Haiyang — Guide & Founder"
- Subline: Descendant of the Pitjara and Iningai people of central Queensland
- Her story (400–500 words): connection to Country, Marrickville community, Diploma of Conservation & Land Management, motivation for starting the tours, what she hopes visitors take away
- Credentials/qualifications: Diploma, First Aid, tour guide certifications, Working with Children
- Acknowledgement of Country (Jessy's own words)
- Small gallery: bush regen site photos, valley photos

---

### 4. Bookings & Contact (`/book`)

**Purpose:** Convert interest into bookings. No payment processing at launch — email/phone inquiry flow.

**Sections:**
- Upcoming tour dates (static list initially, or simple calendar)
- Pricing: per-person rate, group rates, concession/school rates if applicable
- How to book: step-by-step (1. pick a date, 2. email/call, 3. receive confirmation)
- Contact form: name, email, phone, preferred date, group size, message
- Direct contact: jessyhaiyang@gmail.com · 0491 701 488
- What happens next: response time, confirmation process, payment info

---

### 5. Policies (`/policies`)

**Purpose:** Legal/operational clarity. Satisfies insurance and professional requirements.

**Three policy documents (already drafted):**
- Bookings & Refunds Policy
- Safe Work Plan / Workplace Health & Safety Procedures
- Property acknowledgements (Crown Lands NSW)

**Design:** Clean, readable document layout. Dark sidebar nav to jump between policies. Print-friendly.

---

## Technical Approach

- **Stack:** Pure HTML/CSS/JS — no framework, no build step. Single-developer, easy to hand off and maintain.
- **Hosting:** Static file hosting (GitHub Pages, Netlify, or similar) — zero ongoing cost.
- **Booking:** Email/phone inquiry at launch. No payment integration in v1.
- **Images:** Use existing photos (bush regen sites, valley) + Jessy's personal photos. Placeholder cards where photos are pending.
- **Mobile:** Fully responsive. Card grid collapses to single column on mobile.
- **Performance:** Optimised images, minimal JS, fast load on mobile data.
- **Accessibility:** Semantic HTML, sufficient colour contrast, alt text on all images.

---

## Out of Scope (v1)

- Online payment / booking system
- CMS or admin interface
- Blog / Journal section (nav link present but page deferred)
- Multi-language support
- Analytics beyond basic (can add later)
