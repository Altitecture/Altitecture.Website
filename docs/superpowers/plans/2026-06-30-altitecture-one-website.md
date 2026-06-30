# Altitecture One Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Market Altitecture One as a first-class SaaS product on the marketing site via a featured homepage band plus a dedicated `/altitecture-one/` product page.

**Architecture:** Static, build-less site (Tailwind CDN, inline config). Edit `index.html` to add a `#product` band and "Product" nav links; add a new `altitecture-one/index.html` that reuses the same head/theme/scripts. Azure Static Web Apps uploads the repo root as-is, so `altitecture-one/index.html` serves at `/altitecture-one/`.

**Tech Stack:** HTML5, Tailwind CSS via CDN (`?plugins=forms,container-queries`), Space Grotesk + Manrope fonts, Material Symbols, vanilla JS (IntersectionObserver reveal + mobile nav).

## Global Constraints

- Language: Dutch (`lang="nl"`), matching the existing site.
- No price shown anywhere. Primary CTA: "Demo aanvragen" → `mailto:info@altitecture.nl`.
- Product name marketed: **Altitecture One** (umbrella); modules: "MDTO Export & Vernietigingsreviews" and "SharePoint-inrichting".
- Theme tokens come from the inline `tailwind.config` in `index.html` (primary `#89d6b3`, dark surfaces, `clipped-corner`, `borderRadius` default `0px`). Reuse them; do not introduce new colors.
- The product page lives one directory deep, so all its asset references use root-absolute paths (`/images/...`, `/favicon.ico`, `/site.webmanifest`).
- Keep the duplicated `<head>` Tailwind config + `<style>` block byte-identical between the two pages. No build step, no shared-file extraction.
- Do not modify the Portfolio section's three existing cards. Do not push.

---

### Task 1: Homepage product band + "Product" nav links

**Files:**
- Modify: `index.html` (nav desktop ~232-238, nav mobile ~251-258, new `#product` section between `#process` end ~404 and `#portfolio` ~407, footer links ~518-524, keywords meta ~30)

**Interfaces:**
- Produces: an anchor target `#product` on the homepage; a link `/altitecture-one/` consumed by Task 2's page (back-and-forth navigation).

- [ ] **Step 1: Add "Product" to the desktop nav**

In the desktop nav `<div class="hidden md:flex gap-6">`, add between the *Werkwijze* and *Portfolio* links:

```html
<a class="text-on-surface opacity-70 font-headline uppercase tracking-tighter text-sm hover:text-primary focus-visible:text-primary focus-visible:outline focus-visible:outline-primary transition-colors duration-200" href="#product">Product</a>
```

- [ ] **Step 2: Add "Product" to the mobile nav**

In `#mobile-nav`, add between *Werkwijze* and *Portfolio*:

```html
<a class="px-6 py-3 text-on-surface font-headline uppercase tracking-tighter text-sm hover:text-primary hover:bg-surface-container-high focus-visible:text-primary focus-visible:bg-surface-container-high transition-all" href="#product">Product</a>
```

- [ ] **Step 3: Add the `#product` band between `#process` and `#portfolio`**

Insert this section immediately after the closing `</section>` of `#process` and before the `<!-- Portfolio Section -->` comment:

```html
<!-- Product Section: Altitecture One -->
<section id="product" class="reveal py-24 px-6 md:px-16 lg:px-24">
    <div class="max-w-7xl mx-auto">
        <div class="border border-primary/30 bg-surface-container-low/60 p-8 md:p-12 relative overflow-hidden clipped-corner">
            <div class="portfolio-grid absolute inset-0 pointer-events-none"></div>
            <div class="relative z-10 grid grid-cols-1 lg:grid-cols-2 gap-10 items-center">
                <div class="reveal-child">
                    <div class="flex items-center gap-3 text-primary mb-2">
                        <span class="text-[10px] font-bold tracking-[0.3em] uppercase">Ons Eigen Product</span>
                        <div class="h-[1px] flex-grow bg-primary/20"></div>
                    </div>
                    <h2 class="text-4xl md:text-5xl font-headline font-bold tracking-tighter mb-4 text-on-surface">Altitecture One</h2>
                    <p class="text-lg text-on-surface-variant leading-relaxed mb-6">
                        SaaS voor Nederlandse overheidsorganisaties — archiveer SharePoint-documenten naar MDTO en richt nieuwe SharePoint-omgevingen in, conform de Archiefwet.
                    </p>
                    <div class="grid sm:grid-cols-2 gap-4 mb-8">
                        <div class="bg-surface-container-low p-5 border-l-4 border-primary">
                            <h3 class="font-headline text-sm font-bold uppercase tracking-tight mb-2 text-on-surface">MDTO Export &amp; Vernietigingsreviews</h3>
                            <p class="text-sm text-on-surface-variant leading-relaxed">Van Purview-export naar gevalideerd SIP/MDTO-pakket voor het e-Depot, met beoordelingsworkflow.</p>
                        </div>
                        <div class="bg-surface-container-low p-5 border-l-4 border-primary">
                            <h3 class="font-headline text-sm font-bold uppercase tracking-tight mb-2 text-on-surface">SharePoint-inrichting</h3>
                            <p class="text-sm text-on-surface-variant leading-relaxed">Nieuwe sites, Teams en bibliotheken uit herbruikbare blauwdrukken.</p>
                        </div>
                    </div>
                    <div class="flex flex-wrap gap-2 mb-8">
                        <span class="text-[10px] font-bold text-on-surface-variant/60 uppercase border border-outline-variant/20 px-2 py-1">MDTO-standaard</span>
                        <span class="text-[10px] font-bold text-on-surface-variant/60 uppercase border border-outline-variant/20 px-2 py-1">Microsoft 365</span>
                        <span class="text-[10px] font-bold text-on-surface-variant/60 uppercase border border-outline-variant/20 px-2 py-1">Entra ID</span>
                        <span class="text-[10px] font-bold text-on-surface-variant/60 uppercase border border-outline-variant/20 px-2 py-1">e-Depot</span>
                        <span class="text-[10px] font-bold text-on-surface-variant/60 uppercase border border-outline-variant/20 px-2 py-1">Multi-tenant</span>
                    </div>
                    <div class="flex flex-wrap gap-4">
                        <a href="/altitecture-one/" class="inline-block bg-primary text-on-primary font-headline uppercase text-sm font-bold tracking-widest px-8 py-3 clipped-corner active:scale-95 transition-transform hover:bg-primary-fixed focus-visible:ring-2 focus-visible:ring-primary focus-visible:ring-offset-2 focus-visible:ring-offset-surface">Bekijk Altitecture One</a>
                        <a href="mailto:info@altitecture.nl" class="inline-block border border-primary text-primary font-headline uppercase text-sm font-bold tracking-widest px-8 py-3 clipped-corner active:scale-95 transition-transform hover:bg-primary/10 focus-visible:ring-2 focus-visible:ring-primary focus-visible:ring-offset-2 focus-visible:ring-offset-surface">Demo aanvragen</a>
                    </div>
                </div>
                <div class="reveal-child">
                    <div class="aspect-video bg-surface-container-lowest border border-outline-variant/10 relative overflow-hidden">
                        <img src="./images/mdto-export.png" alt="Altitecture One — MDTO Export: SharePoint-documenten omgezet naar MDTO-archiefformaat" class="absolute inset-0 w-full h-full object-cover object-center" />
                    </div>
                </div>
            </div>
        </div>
    </div>
</section>
```

- [ ] **Step 4: Add "Product" to the footer link row**

In the footer `<div class="flex gap-6">`, add between *Werkwijze* and *Portfolio*:

```html
<a class="text-on-surface-variant text-xs uppercase tracking-wider hover:text-primary transition-colors" href="#product">Product</a>
```

- [ ] **Step 5: Extend the keywords meta**

Replace the `keywords` meta content (line ~30) with:

```html
<meta name="keywords" content="AI softwareontwikkeling, maatwerksoftware, webapplicaties, API integratie, dashboards, AI-gedreven development, Rotterdam, Nederland, Altitecture One, MDTO, MDTO export, archiveren, Archiefwet, e-Depot, SharePoint, gemeente, overheid" />
```

- [ ] **Step 6: Verify in a browser**

Open `index.html`. Confirm:
- A "Product" link appears in desktop nav, mobile nav (hamburger), and footer, all between Werkwijze and Portfolio.
- Clicking "Product" scrolls to the Altitecture One band; it renders between Werkwijze and Portfolio with the two module cards, trust chips, the `mdto-export.png` visual, and two working CTAs.
- "Bekijk Altitecture One" points to `/altitecture-one/`; "Demo aanvragen" opens a mail draft to info@altitecture.nl.
- Portfolio still shows exactly the three original cards.

- [ ] **Step 7: Commit**

```bash
git add index.html
git commit -m "feat: add Altitecture One product band and Product nav links to homepage"
```

---

### Task 2: Dedicated product page `/altitecture-one/`

**Files:**
- Create: `altitecture-one/index.html`

**Interfaces:**
- Consumes: the `#product` band's link target `/altitecture-one/` (Task 1); shared theme tokens and scripts copied from `index.html`.
- Produces: a standalone page reachable at `/altitecture-one/` with a back link to `/#product`.

- [ ] **Step 1: Create the page scaffold (head + nav)**

Create `altitecture-one/index.html`. Copy the entire `<head>` from `index.html` verbatim EXCEPT: (a) change `<title>` to `Altitecture One | MDTO-archivering & SharePoint-inrichting`; (b) change the meta description to `Altitecture One — SaaS voor Nederlandse overheid: archiveer SharePoint-documenten naar MDTO en richt SharePoint in, conform de Archiefwet.`; (c) set `<link rel="canonical" href="https://altitecture.nl/altitecture-one/" />` and OG `og:url` to the same; (d) update OG/Twitter title+description to match; (e) change all asset hrefs/srcs from `./images/...`/`./favicon.ico`/`./site.webmanifest` to root-absolute `/images/...`, `/favicon.ico`, `/site.webmanifest`. Keep the `tailwind.config` script and the `<style>` block byte-identical. Replace the two JSON-LD blocks with a single `SoftwareApplication` block:

```html
<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "SoftwareApplication",
    "name": "Altitecture One",
    "applicationCategory": "BusinessApplication",
    "operatingSystem": "Web",
    "url": "https://altitecture.nl/altitecture-one/",
    "publisher": { "@type": "Organization", "name": "Altitecture", "url": "https://altitecture.nl" },
    "description": "Multi-tenant SaaS voor MDTO-archivering vanuit SharePoint en geautomatiseerde SharePoint-inrichting, voor Nederlandse overheidsorganisaties.",
    "inLanguage": "nl"
}
</script>
```

Copy the `<nav>` from `index.html` verbatim, then in BOTH the desktop and mobile nav change the section anchors from `#services`/`#process`/etc. to absolute homepage anchors (`/#services`, `/#process`, `/#product`, `/#portfolio`, `/#about`, `/#contact`) so they work from one level deep. The logo `href="#"` becomes `href="/"`. The hamburger script stays (added in Step 6).

- [ ] **Step 2: Add the sub-hero**

Inside `<main>`, first section:

```html
<section class="relative pt-32 pb-20 px-6 md:px-16 lg:px-24 overflow-hidden">
    <div class="hero-grid absolute inset-0 pointer-events-none"></div>
    <div class="absolute inset-0 bg-gradient-to-b from-surface/80 via-transparent to-surface pointer-events-none"></div>
    <div class="relative z-10 max-w-7xl mx-auto grid grid-cols-1 lg:grid-cols-2 gap-12 items-center">
        <div>
            <div class="flex items-center gap-3 text-primary mb-4">
                <div class="h-[1px] w-12 bg-primary/40"></div>
                <span class="text-[10px] font-bold tracking-[0.3em] uppercase">Altitecture One</span>
            </div>
            <h1 class="text-4xl md:text-6xl font-headline font-bold tracking-tighter mb-6 text-on-surface">MDTO-archivering en SharePoint-beheer, <span class="text-primary">zonder gedoe</span></h1>
            <p class="text-lg text-on-surface-variant leading-relaxed mb-8 max-w-xl">Altitecture One archiveert SharePoint-documenten naar het MDTO-formaat voor het e-Depot en richt nieuwe SharePoint-omgevingen in uit blauwdrukken — gebouwd voor Nederlandse overheidsorganisaties.</p>
            <div class="flex flex-wrap gap-4">
                <a href="mailto:info@altitecture.nl" class="inline-block bg-primary text-on-primary font-headline uppercase text-sm font-bold tracking-widest px-8 py-3 clipped-corner active:scale-95 transition-transform hover:bg-primary-fixed focus-visible:ring-2 focus-visible:ring-primary focus-visible:ring-offset-2 focus-visible:ring-offset-surface">Demo aanvragen</a>
                <a href="/#product" class="inline-block border border-outline-variant/40 text-on-surface-variant font-headline uppercase text-sm font-bold tracking-widest px-8 py-3 clipped-corner hover:text-primary hover:border-primary transition-colors">← Terug naar home</a>
            </div>
        </div>
        <div class="aspect-video bg-surface-container-lowest border border-outline-variant/10 relative overflow-hidden">
            <img src="/images/mdto-export.png" alt="Altitecture One MDTO Export — inlogscherm met MDTO-conceptdiagram" class="absolute inset-0 w-full h-full object-cover object-center" />
        </div>
    </div>
</section>
```

- [ ] **Step 3: Add "Het probleem", both module sections, "Compliance & techniek", "Voor wie", and final CTA**

Append after the sub-hero. Use the established section rhythm (`reveal`, eyebrow line, `max-w-7xl`). Module sections include a clearly-marked screenshot slot.

```html
<!-- Het probleem -->
<section class="reveal py-20 px-6 md:px-16 lg:px-24 bg-surface-container-low">
    <div class="max-w-4xl mx-auto">
        <div class="flex items-center gap-3 text-primary mb-2"><span class="text-[10px] font-bold tracking-[0.3em] uppercase">De Uitdaging</span><div class="h-[1px] flex-grow bg-primary/20"></div></div>
        <h2 class="text-3xl md:text-4xl font-headline font-bold tracking-tighter mb-6 text-on-surface">Archiveren conform de Archiefwet is complex</h2>
        <p class="text-lg text-on-surface-variant leading-relaxed mb-4">Overheidsorganisaties moeten informatie duurzaam toegankelijk bewaren of op tijd vernietigen — volgens de Selectielijst en in het MDTO-formaat van het Nationaal Archief. In de praktijk staat die informatie verspreid in SharePoint en Microsoft 365.</p>
        <p class="text-lg text-on-surface-variant leading-relaxed">Handmatig dossiers beoordelen, metadata mappen en SIP-pakketten samenstellen voor het e-Depot is foutgevoelig en tijdrovend. Altitecture One automatiseert die keten — met behoud van een volledige audit-trail.</p>
    </div>
</section>

<!-- Module 1: MDTO Export -->
<section class="reveal py-20 px-6 md:px-16 lg:px-24">
    <div class="max-w-7xl mx-auto grid grid-cols-1 lg:grid-cols-2 gap-12 items-center">
        <div class="reveal-child">
            <div class="flex items-center gap-3 text-primary mb-2"><span class="text-[10px] font-bold tracking-[0.3em] uppercase">Module</span><div class="h-[1px] flex-grow bg-primary/20"></div></div>
            <h2 class="text-3xl md:text-4xl font-headline font-bold tracking-tighter mb-4 text-on-surface">MDTO Export &amp; Vernietigingsreviews</h2>
            <p class="text-on-surface-variant leading-relaxed mb-6">Importeer een Purview-dispositie-export, laat dossiers beoordelen door de juiste rollen, en lever gevalideerde MDTO/SIP-pakketten op voor het e-Depot.</p>
            <ul class="space-y-3">
                <li class="flex gap-3 text-on-surface-variant"><span class="material-symbols-outlined text-primary text-xl">upload_file</span><span>Import van Purview-dispositie-CSV naar dossiers en series.</span></li>
                <li class="flex gap-3 text-on-surface-variant"><span class="material-symbols-outlined text-primary text-xl">how_to_reg</span><span>Beoordelingsworkflow: Informatiebeheer → Data-eigenaar → Archivaris → vaststellen.</span></li>
                <li class="flex gap-3 text-on-surface-variant"><span class="material-symbols-outlined text-primary text-xl">archive</span><span>Automatische MDTO-XML en SIP-pakketten, conform de Selectielijst.</span></li>
            </ul>
        </div>
        <div class="reveal-child">
            <!-- SCREENSHOT SLOT: vervang door echte schermafbeelding (met data) van Vernietigingsreviews -->
            <div class="aspect-video bg-surface-container-lowest border border-outline-variant/10 flex items-center justify-center clipped-corner">
                <div class="text-center px-6">
                    <span class="material-symbols-outlined text-primary text-4xl mb-3">account_tree</span>
                    <p class="text-on-surface-variant text-sm">Purview CSV → Dossier → Beoordeling → SIP/MDTO → e-Depot</p>
                </div>
            </div>
        </div>
    </div>
</section>

<!-- Module 2: SharePoint-inrichting -->
<section class="reveal py-20 px-6 md:px-16 lg:px-24 bg-surface-container-low">
    <div class="max-w-7xl mx-auto grid grid-cols-1 lg:grid-cols-2 gap-12 items-center">
        <div class="reveal-child order-2 lg:order-1">
            <!-- SCREENSHOT SLOT: vervang door echte schermafbeelding (met data) van Site-inrichting -->
            <div class="aspect-video bg-surface-container-lowest border border-outline-variant/10 flex items-center justify-center clipped-corner">
                <div class="text-center px-6">
                    <span class="material-symbols-outlined text-primary text-4xl mb-3">dashboard_customize</span>
                    <p class="text-on-surface-variant text-sm">Blauwdruk → Site + Team + bibliotheken</p>
                </div>
            </div>
        </div>
        <div class="reveal-child order-1 lg:order-2">
            <div class="flex items-center gap-3 text-primary mb-2"><span class="text-[10px] font-bold tracking-[0.3em] uppercase">Module</span><div class="h-[1px] flex-grow bg-primary/20"></div></div>
            <h2 class="text-3xl md:text-4xl font-headline font-bold tracking-tighter mb-4 text-on-surface">SharePoint-inrichting</h2>
            <p class="text-on-surface-variant leading-relaxed mb-6">Richt consistente SharePoint-omgevingen in vanuit herbruikbare blauwdrukken — sites, Teams, bibliotheken, contenttypes en weergaven, in één gecontroleerde stap.</p>
            <ul class="space-y-3">
                <li class="flex gap-3 text-on-surface-variant"><span class="material-symbols-outlined text-primary text-xl">content_copy</span><span>Blauwdrukken op basis van een referentiesite.</span></li>
                <li class="flex gap-3 text-on-surface-variant"><span class="material-symbols-outlined text-primary text-xl">groups</span><span>Nieuwe Microsoft Team + site + standaardkanalen.</span></li>
                <li class="flex gap-3 text-on-surface-variant"><span class="material-symbols-outlined text-primary text-xl">checklist</span><span>Idempotente, herhaalbare provisioning met statusoverzicht.</span></li>
            </ul>
        </div>
    </div>
</section>

<!-- Compliance & techniek -->
<section class="reveal py-20 px-6 md:px-16 lg:px-24">
    <div class="max-w-7xl mx-auto">
        <div class="flex items-center gap-3 text-primary mb-2"><span class="text-[10px] font-bold tracking-[0.3em] uppercase">Compliance &amp; Techniek</span><div class="h-[1px] flex-grow bg-primary/20"></div></div>
        <h2 class="text-3xl md:text-4xl font-headline font-bold tracking-tighter mb-10 text-on-surface">Gebouwd op standaarden</h2>
        <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
            <div class="reveal-child bg-surface-container-low p-6 border-l-4 border-primary"><h3 class="font-headline text-sm font-bold uppercase tracking-tight mb-2 text-on-surface">MDTO &amp; Selectielijst</h3><p class="text-sm text-on-surface-variant leading-relaxed">Output conform de MDTO-standaard van het Nationaal Archief en de gemeentelijke Selectielijst.</p></div>
            <div class="reveal-child bg-surface-container-low p-6 border-l-4 border-primary"><h3 class="font-headline text-sm font-bold uppercase tracking-tight mb-2 text-on-surface">Microsoft 365 &amp; Entra ID</h3><p class="text-sm text-on-surface-variant leading-relaxed">Inloggen met uw eigen Microsoft-account; veilige toegang tot SharePoint via Entra ID.</p></div>
            <div class="reveal-child bg-surface-container-low p-6 border-l-4 border-primary"><h3 class="font-headline text-sm font-bold uppercase tracking-tight mb-2 text-on-surface">Multi-tenant &amp; audit</h3><p class="text-sm text-on-surface-variant leading-relaxed">Strikte scheiding per organisatie (row-level security) en een volledige, doorzoekbare audit-trail.</p></div>
        </div>
    </div>
</section>

<!-- Voor wie -->
<section class="reveal py-20 px-6 md:px-16 lg:px-24 bg-surface-container-low">
    <div class="max-w-4xl mx-auto text-center">
        <div class="flex items-center justify-center gap-3 text-primary mb-2"><span class="text-[10px] font-bold tracking-[0.3em] uppercase">Voor Wie</span></div>
        <h2 class="text-3xl md:text-4xl font-headline font-bold tracking-tighter mb-6 text-on-surface">Voor gemeenten en overheidsorganisaties</h2>
        <p class="text-lg text-on-surface-variant leading-relaxed">Altitecture One is gemaakt voor gemeenten, overheidsorganisaties en archiefinstellingen die Microsoft 365 gebruiken en hun informatiehuishouding aantoonbaar op orde willen brengen.</p>
    </div>
</section>

<!-- CTA -->
<section class="reveal py-24 px-6 md:px-16 lg:px-24 bg-primary-container relative overflow-hidden">
    <div class="absolute top-0 right-0 w-48 h-48 bg-on-primary-container/5 -mr-24 -mt-24 rotate-45"></div>
    <div class="max-w-7xl mx-auto relative z-10 text-center">
        <h2 class="text-4xl md:text-5xl font-headline font-bold tracking-tighter mb-4 text-on-primary-container">Benieuwd wat Altitecture One voor u kan doen?</h2>
        <p class="text-lg text-on-primary-container/70 mb-10 max-w-xl mx-auto">Plan een demo — we laten zien hoe u vanuit SharePoint naar een gevalideerd MDTO-archief komt.</p>
        <a href="mailto:info@altitecture.nl" class="inline-block bg-on-primary-container text-primary-container font-headline uppercase text-sm font-bold tracking-widest px-10 py-3 clipped-corner active:scale-95 transition-transform hover:opacity-90 focus-visible:ring-2 focus-visible:ring-on-primary-container focus-visible:ring-offset-2 focus-visible:ring-offset-primary-container">Demo aanvragen</a>
    </div>
</section>
```

- [ ] **Step 4: Add the footer**

Copy the `<footer>` from `index.html`, but change the section anchor hrefs to absolute homepage anchors (`/#services`, `/#product`, `/#portfolio`, `/#about`, `/#contact`) and `#process` → `/#process`. Keep logo image at `/images/logo.png`.

- [ ] **Step 5: Add the visual-accent divs + scroll/mobile-nav script**

Copy verbatim from `index.html`: the two `<!-- Visual Accents -->` fixed divs and the entire `<!-- Scroll Reveal -->` `<script>` (IntersectionObserver + mobile-nav toggle). Close `</body></html>`.

- [ ] **Step 6: Verify in a browser**

Serve the site root (`npx serve .`) and open `/altitecture-one/`. Confirm:
- Page matches the site theme (dark, green accent, fonts, clipped corners).
- All seven content blocks render: sub-hero, probleem, two modules (with screenshot-slot placeholders), compliance, voor wie, CTA.
- Mobile hamburger opens/closes; reveal-on-scroll animates sections.
- Nav links go to `/#...` homepage anchors; logo → `/`; "← Terug naar home" → `/#product`; all "Demo aanvragen" CTAs open mail to info@altitecture.nl.
- No price anywhere. No broken images (the `mdto-export.png` loads from `/images/`).

- [ ] **Step 7: Commit**

```bash
git add altitecture-one/index.html
git commit -m "feat: add dedicated Altitecture One product page"
```

---

### Task 3: Cross-page verification

**Files:** none (verification only)

- [ ] **Step 1: Round-trip navigation check**

Serve the root (`npx serve .`). From `index.html`: click "Product" (nav) → scrolls to band; click "Bekijk Altitecture One" → lands on `/altitecture-one/`; from there click "← Terug naar home" and a nav link → returns to the homepage and the correct anchor. Confirm desktop and mobile (narrow viewport) both work.

- [ ] **Step 2: Final visual pass + commit any fixes**

Screenshot both pages at desktop and mobile widths; confirm the product band is visually distinct from Werkwijze/Portfolio and the product page reads as a cohesive whole. If any spacing/anchor fixes are needed, make them and commit:

```bash
git add -A
git commit -m "fix: Altitecture One page/band polish"
```

---

## Self-Review

- **Spec coverage:** Homepage band (Task 1.3) ✓; Product nav desktop/mobile/footer (1.1, 1.2, 1.4) ✓; dedicated `/altitecture-one/` page with all 7 sections (Task 2) ✓; on-brand graphics + screenshot slots (2.2, 2.3) ✓; no price / email CTA (global constraint, enforced in every CTA) ✓; SEO/canonical/OG/JSON-LD (2.1) ✓; keywords extended (1.5) ✓; routing via folder index, root-absolute paths (global + 2.1) ✓; Portfolio untouched (constraint, verified 1.6) ✓; reuse reveal/mobile-nav/reduced-motion scripts (2.5) ✓.
- **Placeholder scan:** The only "placeholders" are intentional, content-bearing screenshot SLOT blocks (real HTML, marked for later replacement) — not plan placeholders. No TBD/TODO in steps.
- **Type/name consistency:** Anchor `#product` defined in 1.3, linked in 1.1/1.2/1.4/2.x; page path `/altitecture-one/` consistent between Task 1 link and Task 2 file location; module names identical across band and page ("MDTO Export & Vernietigingsreviews", "SharePoint-inrichting").
