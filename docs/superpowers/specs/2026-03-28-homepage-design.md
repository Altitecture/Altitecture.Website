# Altitecture Homepage Design Spec

## Overview

A single-page static homepage for Altitecture, a new software company that uses AI to build custom software for customers. The homepage establishes brand credibility with a technical, precise tone aimed at CTOs, engineers, and technical decision-makers.

## Tech Stack

- Static HTML + Tailwind CSS (CDN)
- No build tools, no JS frameworks
- CSS animations + vanilla JS Intersection Observer for scroll reveals
- File: `homepage.html` (separate from reference `index.html`)

## Design System (from Stitch reference)

- **Background:** `#131313` (surface)
- **Primary accent:** `#89d6b3` (emerald green)
- **Surface layers:** `#1c1b1b` (low), `#201f1f` (container), `#2a2a2a` (high), `#353534` (highest)
- **Text:** `#e5e2e1` (on-surface), `#bfc9c3` (on-surface-variant)
- **Outline:** `#89938d`, variant `#404944`
- **Headlines:** Space Grotesk (bold, tracking-tighter)
- **Body/Labels:** Manrope
- **Border radius:** 0px (sharp corners throughout)
- **Buttons:** Clipped-corner polygon (`clip-path: polygon(0 0, 90% 0, 100% 20%, 100% 100%, 0 100%)`)
- **Icons:** Material Symbols Outlined
- **Accent patterns:** Border-left highlights, dot-grid backgrounds, uppercase tracking-widest labels

## Layout

Immersive/cinematic single-page layout. Full-screen hero with animated background. Subsequent sections reveal on scroll via Intersection Observer + CSS fade/slide-up animations.

## Fixed Navigation

- Fixed top bar matching Stitch reference (h-16, `#131313` background)
- Logo (logo.png) + "Altitecture" wordmark (mixed case, Space Grotesk)
- Nav links: smooth-scroll anchors to each section (Services, Process, Portfolio, About, Contact)
- Right side: "Get Started" clipped-corner CTA button

## Sections

### 1. Hero (full viewport)

- `100vh` height, vertically and horizontally centered content
- **Background:** Animated dot-grid pattern using CSS `radial-gradient` with subtle pulsing/floating keyframe animation
- **Headline:** ~48-64px Space Grotesk bold, e.g. "We build software with AI"
- **Tagline:** Manrope, muted color, one line, e.g. "Custom software, built faster than you thought possible"
- **CTA:** Emerald clipped-corner button, e.g. "Get Started"
- **Scroll indicator:** Animated chevron icon at bottom of viewport, bouncing

### 2. Services / What We Build

- Section label: uppercase tracking-widest breadcrumb ("What We Build") with horizontal rule
- **3-column grid** (responsive: stacks on mobile)
- Each card: `surface-container-low` background, `border-l-4 border-primary` accent
  - Material Symbol icon (emerald)
  - Title (Space Grotesk, bold, uppercase)
  - 1-2 line description (Manrope, muted)
- Example services:
  - Web Applications — "Full-stack web apps tailored to your workflow"
  - APIs & Integrations — "Connect your systems with robust, scalable APIs"
  - Dashboards & Analytics — "Real-time data visualization and business intelligence"

### 3. How It Works / Process

- Section label + headline
- **Horizontal timeline** with 4 numbered steps connected by emerald lines
- Each step: numbered square (border, emerald text) + title + one-line description
- Steps:
  1. Brief — "Tell us what you need"
  2. Architect — "We design the solution"
  3. Build — "AI-powered development"
  4. Ship — "Deployed and supported"
- Timeline connectors: horizontal lines between step markers
- Responsive: vertical stack on mobile

### 4. Portfolio / Case Studies

- Section label + headline
- **2-column card grid** (stacks on mobile)
- Each card:
  - Image area: `surface-container-lowest` with dot-grid background (placeholder)
  - Title (Space Grotesk)
  - Short description
  - Tech tags: small bordered pills (e.g. "React", "Node.js", "PostgreSQL")
- 2-4 placeholder projects with realistic-sounding titles

### 5. About

- Section label + headline
- **Split layout:** text (left ~60%) + visual element (right ~40%)
- Text: 2-3 sentences about AI-first approach, company mission
- Visual: geometric/abstract element or logo mark, styled with emerald accents
- Keep minimal — credibility through brevity

### 6. CTA Block

- Full-width `primary-container` background (`#2e7c5e`)
- Headline: "Ready to build?" (Space Grotesk, `on-primary-container` color)
- Subtext: one line
- Large clipped-corner button: "Book a Call" or "Get in Touch"
- Decorative rotated square in corner (matching Stitch System Health card style)

### 7. Footer

- `surface-container-low` background
- Logo + wordmark (left)
- Nav links row (center or right)
- Copyright line
- Minimal — no social icons unless needed later

## Scroll Reveal Animation

- Intersection Observer watches each section
- When section enters viewport (threshold ~0.1): add `.revealed` class
- CSS transition: `opacity 0→1`, `translateY(30px→0)`, duration ~0.6s, ease-out
- Sections start with `opacity: 0; transform: translateY(30px)`
- Stagger child elements slightly for cascade effect

## Responsive Behavior

- **Desktop (1024px+):** Full layout as described
- **Tablet (768-1023px):** 2-column grids become 2-col, process stays horizontal
- **Mobile (<768px):** Single column, hero text smaller (~36px), process becomes vertical, nav collapses to hamburger menu

## Files

- `homepage.html` — the homepage
- `logo.png` — existing logo asset (already downloaded)
- `index.html` — kept as-is (reference only, not published)
