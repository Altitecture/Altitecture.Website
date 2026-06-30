# Altitecture One — Website Placement Design

**Date:** 2026-06-30
**Status:** Approved (design), pending implementation plan
**Scope:** `Altitecture.Website` (the marketing site) only. No changes to the product repo.

## Summary

Altitecture One is a multi-tenant SaaS for Dutch (public-sector) information governance on
Microsoft 365 / SharePoint. It is a recurring-revenue **product** with its own buyer
(information managers, archivists, M365 admins at municipalities) — fundamentally different
from the one-off client work in the current Portfolio.

The marketing site today positions Altitecture purely as an AI-driven **services** company.
This change introduces Altitecture One as a first-class product without disrupting that
services story: a featured band on the homepage plus a dedicated product page.

It is deliberately **not** added as a fourth Portfolio card — that would frame a live SaaS as
finished client work and bury it among unrelated consumer apps.

## Decisions (locked)

| Decision | Choice |
|---|---|
| Prominence | Featured homepage band **and** a dedicated product page |
| Product name | **Altitecture One** (umbrella), with two modules |
| Modules | "MDTO Export & Vernietigingsreviews" and "SharePoint-inrichting" |
| Visuals | On-brand dark graphics now; marked screenshot slots for later |
| Primary CTA | "Demo aanvragen" → `mailto:info@altitecture.nl` |
| Price | Not shown anywhere |
| Language | Dutch (matches the rest of the site, `lang="nl"`) |

## What Altitecture One is (reference)

Source: `C:\Projects\Altitecture\Altitecture.SharePointMdtoExport` (`CONTEXT.md`, `CLAUDE.md`).

- Multi-tenant SaaS. .NET 10 / ASP.NET Core / PostgreSQL / Azure Blob; Entra ID (multi-tenant)
  auth; PnP.Core + Microsoft Graph; SignalR; row-level security per tenant.
- **Module 1 — MDTO Export & Disposition Review (Vernietigingsreviews):** imports a Microsoft
  Purview disposition CSV, runs a review workflow (Information Management → Data Owner →
  Archivist → finalize), and packages SharePoint documents into SIP/MDTO XML packages for
  delivery to an e-Depot. Conforms to the Nationaal Archief **MDTO** standard and the
  **Selectielijst**.
- **Module 2 — SharePoint Provisioning (SharePoint-inrichting):** provisions new SharePoint
  sites + Microsoft Teams + configured document libraries from reusable **Blueprints**
  (blauwdrukken).
- Target customers: Dutch municipalities, government bodies, and archival institutions subject
  to the Archiefwet that run Microsoft 365 / SharePoint.

Naming note: the app UI currently brands itself "Altitecture MDTO Export". The website markets
the umbrella name **Altitecture One**; aligning the in-app title is out of scope here.

## Information architecture

- **Homepage band `#product`** inserted **after `#process` (Werkwijze), before `#portfolio`**.
- **Nav item "Product"** added to desktop nav, mobile nav, and footer — between *Werkwijze* and
  *Portfolio*. Links to `#product` on the homepage; the band's primary CTA links to the page.
- **Dedicated page** at `/altitecture-one/` served by `altitecture-one/index.html`.
- **Portfolio unchanged** — the three existing client cards stay; Altitecture One is not among
  them.

## Homepage band (`#product`)

Visually distinct from its neighbours so it reads as a product, not another services/portfolio
block: a bordered, elevated **dark** treatment (not the flat `primary-container` used by the
Contact section), reusing the angular clipped-corner styling and the dark MDTO diagram visual.

Content (Dutch, draft — final copy refined during implementation):

- Eyebrow: `ONS EIGEN PRODUCT`
- H2: **Altitecture One**
- Lead: "SaaS voor Nederlandse overheidsorganisaties — archiveer SharePoint-documenten naar
  MDTO en richt nieuwe SharePoint-omgevingen in, conform de Archiefwet."
- Two module cards:
  - **MDTO Export & Vernietigingsreviews** — "Van Purview-export naar gevalideerd
    SIP/MDTO-pakket voor het e-Depot, met beoordelingsworkflow."
  - **SharePoint-inrichting** — "Nieuwe sites, Teams en bibliotheken uit herbruikbare
    blauwdrukken."
- Trust chips: `MDTO-standaard` · `Microsoft 365` · `Entra ID` · `e-Depot` · `Multi-tenant`
- CTAs: **Bekijk Altitecture One** (→ `/altitecture-one/`) and **Demo aanvragen**
  (→ `mailto:info@altitecture.nl`).
- No price.

## Dedicated page `/altitecture-one/`

Shares the same nav, footer, fonts, Tailwind config, and custom styles as the homepage. The nav
"Product" link returns to `/#product` (or a "← Terug" home link). Sections, in order:

1. **Sub-hero** — eyebrow "Altitecture One", H1 (e.g. "MDTO-archivering en SharePoint-beheer,
   zonder gedoe"), lead paragraph, CTA *Demo aanvragen* + back-to-home link. Dark diagram visual
   (`/images/mdto-export.png`).
2. **Het probleem** — Archiefwet / Selectielijst / e-Depot pressure on municipalities using
   Microsoft 365.
3. **Module — MDTO Export & Vernietigingsreviews** — the flow: Purview CSV → dossiers →
   Information Management → Data Owner → Archivist → finalize → SIP/MDTO XML → e-Depot. On-brand
   graphic + a marked screenshot slot.
4. **Module — SharePoint-inrichting** — Blauwdruk → nieuwe Site + Team + bibliotheken. On-brand
   graphic + a marked screenshot slot.
5. **Compliance & techniek** — MDTO-standaard (Nationaal Archief), Selectielijst, Microsoft 365 /
   Entra ID, multi-tenant met row-level security, volledige audit-trail.
6. **Voor wie** — gemeenten, overheidsorganisaties, archiefinstellingen.
7. **CTA** — *Demo aanvragen* → `mailto:info@altitecture.nl`. No price.

## Visuals / assets plan

- Reuse `images/mdto-export.png` (dark diagram) as the page hero visual.
- New on-brand graphics built with HTML/CSS/inline SVG in the site's dark/green style (an MDTO
  XML snippet card, a SharePoint→MDTO flow, a disposition-workflow diagram) — no new binary image
  dependencies.
- Each module section includes a clearly-commented screenshot placeholder
  (`<!-- SCREENSHOT SLOT: ... -->`) sized to drop in real captures with data later.
- The current product screenshots (under the product repo's `handleiding/`) are **not** used —
  they are empty-state and light-theme, off-brand for this dark site.
- A dedicated product OG image is a later slot; until then OG falls back to the existing
  `images/og-image.png`.

## Technical / SEO

- **Routing:** Azure Static Web Apps uploads the repo root as static (`app_location: "/"`,
  `skip_app_build: true`). `altitecture-one/index.html` serves at `/altitecture-one/` with no
  `staticwebapp.config.json` needed.
- **Asset paths:** the product page uses root-absolute paths (`/images/...`, `/favicon.ico`)
  because it lives one directory deep.
- **Head duplication:** the page duplicates the inline Tailwind CDN config and custom `<style>`
  block (the site has no build step). Keep them byte-identical to the homepage. Extracting a
  shared head/CSS is explicitly out of scope.
- **SEO:** own `<title>`, meta description, `canonical` (`https://altitecture.nl/altitecture-one/`),
  Open Graph + Twitter tags, and `SoftwareApplication` JSON-LD structured data **without**
  `offers`/price. Extend the homepage `keywords` with MDTO, archiveren, gemeente, e-Depot,
  SharePoint.
- **Consistency:** reuse the existing reveal-on-scroll IntersectionObserver, mobile-nav toggle
  script, and `prefers-reduced-motion` handling on the new page.

## Out of scope (now)

- Demo request form and any backend/form handler (email CTA only).
- Pricing of any kind.
- Real product screenshots (placeholders only).
- English-language version of the product page.
- Extracting shared head/CSS between the two HTML pages.
- Renaming the in-app "Altitecture MDTO Export" branding to "Altitecture One".

## Success criteria

- Homepage shows a distinct Altitecture One product band between Werkwijze and Portfolio, with
  working "Product" nav links (desktop, mobile, footer) and a CTA through to `/altitecture-one/`.
- `/altitecture-one/` renders all seven sections, matches the site's theme exactly, and works on
  mobile and desktop with the same animations/nav behaviour.
- No price appears anywhere; primary CTA is "Demo aanvragen" → `mailto:info@altitecture.nl`.
- Portfolio still shows exactly the three existing client cards.
- Both pages pass a visual check in a browser; links and anchors resolve correctly.
