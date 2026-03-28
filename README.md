# Altitecture Website

Marketing website for [Altitecture](mailto:info@altitecture.nl) — an AI-first software development company.

## Overview

A single-page static website built with Tailwind CSS. Sections include:

- **Hero** — animated dot-grid background with primary CTA
- **Services** — web apps, APIs & integrations, dashboards & analytics
- **Process** — four-step timeline (Brief, Architect, Build, Ship)
- **Portfolio** — project showcase cards
- **About** — company description
- **Contact** — email CTA

## Tech Stack

- HTML5 (single `index.html`)
- [Tailwind CSS](https://tailwindcss.com/) via CDN
- [Space Grotesk](https://fonts.google.com/specimen/Space+Grotesk) (headlines) + [Manrope](https://fonts.google.com/specimen/Manrope) (body)
- [Material Symbols](https://fonts.google.com/icons) for icons
- Vanilla JS for scroll-reveal animations and mobile nav

## Running Locally

Open `index.html` in a browser. No build step required.

For local development with live reload:

```bash
npx serve .
```

## Design

- Dark theme with green (`#89d6b3`) primary accent
- Sharp-cornered, angular aesthetic (clipped corners, no border radius)
- Responsive with mobile hamburger nav
- Respects `prefers-reduced-motion`
