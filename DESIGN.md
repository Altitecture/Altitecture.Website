# Design System: Altitecture Landing Page
**Project ID:** 11669170042721033126

## 1. Visual Theme & Atmosphere

**Creative North Star: "The Synthetic Architect"**

The atmosphere is **dark, architectural, and cinematic** — a high-end editorial experience that treats the interface as a blueprint for AI intelligence. The aesthetic marries algorithmic precision with organic fluidity, drawing from the triangular geometry and sweeping curves of the Altitecture logo.

Expect **intentional asymmetry**: vast pools of negative space punctuated by dense clusters of information. Overlapping elements suggest depth and sophisticated layering. The mood is that of a premium control room — not a generic SaaS dashboard, but something closer to a luxury automotive HUD rendered in obsidian and living teal.

**Key Atmosphere Words:** Dark, Architectural, Cinematic, Precise, Premium, Fluid, Geometric

---

## 2. Color Palette & Roles

### Core Palette

| Descriptive Name | Hex | Role |
|---|---|---|
| **Obsidian Void** | `#0f1413` | Primary background (`surface`, `background`). The deepest layer of the UI. |
| **Living Teal** | `#00e0ba` | Primary accent (`primary`). Used for CTAs, active states, data highlights, and the signature "pulse" of the interface. |
| **Deep Emerald** | `#00a589` | Primary container. Used in gradients with Living Teal (135deg) for hero CTAs and key moments. |
| **Muted Sage** | `#a9cec6` | Secondary text and subtle accents (`secondary`). Provides a softer, humanist counterpoint to the vibrant teal. |
| **Dark Teal Basin** | `#2d4f49` | Secondary container. Used for secondary button backgrounds and quiet interactive states. |
| **Warm Ember** | `#ffb4a6` | Tertiary accent (`tertiary`). A warm coral counterpoint — used sparingly for contrast and warmth signals. |
| **Faded Terracotta** | `#c68073` | Tertiary container. Supporting warm accent for badges or status indicators. |

### Surface Hierarchy (Obsidian Layers)

Treat the UI as stacked sheets of darkened obsidian. Depth is created by shifting between these tiers — **never by 1px solid borders**.

| Descriptive Name | Hex | Role |
|---|---|---|
| **Deepest Obsidian** | `#0a0f0e` | `surface-container-lowest` — Recessed elements, chart backgrounds, "hardware" feel |
| **Deep Layer** | `#181d1b` | `surface-container-low` — Section backgrounds, alternating content bands |
| **Mid Layer** | `#1c211f` | `surface-container` — Card backgrounds, default containers |
| **Raised Layer** | `#262b2a` | `surface-container-high` — Elevated cards, hover states, input fields |
| **Topmost Layer** | `#313634` | `surface-container-highest` — Maximum surface elevation, input tracks |

### Text & Interface Colors

| Descriptive Name | Hex | Role |
|---|---|---|
| **Soft Platinum** | `#dfe3e1` | Primary text (`on-surface`). Never pure white — maintains sophisticated muted tone. |
| **Muted Silver** | `#bfc9c4` | Secondary/descriptive text (`on-surface-variant`). Body copy and supporting content. |
| **Faint Boundary** | `#3f4946` | `outline-variant` — Ghost borders (max 15-20% opacity), subtle dividers. |
| **Subdued Slate** | `#89938f` | `outline` — Placeholder text, disabled states. |
| **Deep Teal Ink** | `#00382d` | Text on primary backgrounds (`on-primary`). |

### Signature Color Rules

- **No-Line Rule:** 1px solid borders are prohibited for sectioning. Use background color shifts between surface tiers instead.
- **Ghost Border Fallback:** If a border is essential, use `outline-variant` at 15-20% opacity. Full-opacity borders are forbidden.
- **Teal Glows:** Large radial gradients of `primary` at 5-8% opacity in the far background suggest an "AI brain" pulsing behind the UI.
- **Glass & Gradient:** Floating elements use glassmorphism — semi-transparent `surface-variant` (~40% opacity) with 20-40px backdrop-blur.
- **Hero Gradient:** `linear-gradient(135deg, #00e0ba 0%, #00a589 100%)` for primary CTAs and hero moments.

---

## 3. Typography Rules

### Font Stack

| Voice | Family | Purpose |
|---|---|---|
| **Technical Voice** | Space Grotesk | Headlines, display text, labels, navigation, buttons |
| **Interface Voice** | Manrope | Body copy, descriptions, paragraphs |

### Usage Guidelines

- **Display/Hero (Space Grotesk):** Extra-bold to black weight (`font-black`). Tight tracking (`tracking-tighter`, approximately -2%). Line height compressed to `leading-[0.9]` for dense editorial authority. Sizes range from `text-4xl` to `text-8xl`.
- **Section Headers (Space Grotesk):** Bold to extra-bold. `text-2xl` to `text-6xl`. Tight tracking. Multi-line headers use `<br/>` for deliberate line breaks.
- **Body (Manrope):** Regular to medium weight (`400-500`). Generous line height (`leading-relaxed`). `text-lg` to `text-2xl` for primary content. Ensures the experience feels premium and accessible.
- **Labels & Micro-copy (Space Grotesk):** Uppercase with wide letter-spacing (`tracking-widest`, `tracking-[0.2em]`). `text-xs` to `text-sm`. Mimics architectural notation.
- **Never use pure white** (`#ffffff`) for text. Always use `on-surface` (`#dfe3e1`) or lighter semantic tokens.

---

## 4. Component Stylings

### Buttons

- **Primary CTA:** Hero gradient background (`linear-gradient 135deg, #00e0ba to #00a589`). Deep teal ink text (`#00382d`). "Signature Curve" shape — three corners at `0.375rem`, top-right at `0.75rem`. Font: Space Grotesk, black weight. Often includes a trailing arrow icon. Diffused teal shadow: `shadow-[0_0_40px_rgba(0,224,186,0.2)]`.
- **Secondary:** Raised Layer background (`#262b2a`) with ghost border (`outline-variant` at 30% opacity). Soft Platinum text. Signature Curve shape. Hover shifts to Topmost Layer.
- **Tertiary/Text:** Text-only using Living Teal color. Accompanied by a directional icon. Group-hover translates the icon.
- **Interaction:** `scale-95` base with `active:scale-90` press feedback. Smooth `transition-transform`.

### Cards & Containers

- **Standard Cards:** Mid Layer background (`#1c211f`). Signature Curve corners. Left accent border in `primary` at 30% opacity, transitioning to full `primary` on hover. Internal padding of `p-8`.
- **Glass Cards:** Semi-transparent `surface-variant` at 40% opacity with 40px backdrop-blur. Generous rounding (`rounded-2xl`). Faint ghost border (`outline-variant` at 20% opacity).
- **Feature Highlight Cards:** `primary-container` at 20% opacity background with `primary` ghost border at 10% opacity. Used for stats and callouts.
- **The "No Divider" Rule:** No horizontal divider lines. Separate content using vertical whitespace or background tier shifts.
- **Hover States:** Cards shift one surface tier higher. Ghost borders may appear or intensify.

### Input Fields & Chat

- **Input Track:** `surface-container-lowest` background. Pill-shaped (`rounded-full`). Subtle `outline-variant` border. Contains icon buttons at each end.
- **Focus Accent:** 2px `primary` accent tab on the left side only — never a full outline.
- **Chat Bubbles:** AI responses use `surface-container-highest` with Signature Curve. User responses use `primary` at 10% opacity with `primary` border at 20% opacity.

### Icons

- **Material Symbols Outlined** (variable weight/fill). Default: weight 400, fill 0.
- Primary icons use Living Teal color. Size varies: `text-sm` for inline, `text-3xl` for feature cards, up to `text-[12rem]` for decorative watermarks at low opacity.
- Feature card icons sit inside `w-14 h-14` containers with `primary-container` at 20% opacity, transitioning to 40% on group hover.

### Navigation

- **Top Bar:** Fixed, full-width. Background: dark emerald at 80% opacity with `backdrop-blur-xl`. Max-width container centered.
- **Nav Links:** Uppercase Space Grotesk labels with widest tracking. Active state: Living Teal with 2px bottom border. Inactive: muted emerald at 70% opacity.
- **Logo:** Space Grotesk, extra-black weight, tighter tracking, Living Teal color.

---

## 5. Layout Principles

### Spacing & Grid

- **Max Content Width:** `max-w-screen-2xl` (~1536px), centered with `mx-auto`.
- **Section Padding:** Generous vertical rhythm — `py-20` to `py-32` between major sections. Horizontal padding `px-8` base.
- **Grid System:** 12-column grid (`grid-cols-12`) for complex layouts. Common splits: 7/5 (hero), 8/4 (bento), 6/6 (showcase).
- **Spacing Scale:** Use the project's spacing scale rigorously. When a screen feels busy, increase padding using `16` or `20` tokens. High-end design lives in the precision of its margins.
- **Gap:** `gap-8` to `gap-12` between grid items. `gap-4` to `gap-6` for smaller element groups.

### Depth & Elevation

- **Tonal Layering:** Depth is achieved by stacking surface tiers, not by drop shadows. A card on `surface-container` placed over a `surface-container-low` section creates natural lift.
- **Ambient Shadows:** When floating effects are required, use extra-diffused shadows (blur: 30px+, spread: -5px) at low opacity. Shadow color is teal-tinted, never pure black.
- **Glassmorphism:** Reserved for floating and overlay elements. Semi-transparent backgrounds with 20-40px backdrop-blur.

### Signature Motifs

- **Signature Curve:** The defining geometric motif — three standard-rounded corners with one generously-rounded top-right corner. Applied to buttons, cards, and key containers. Evokes the logo's triangular movement.
- **Teal Glow Backgrounds:** Fixed-position, large radial gradients of `primary` at 5-8% opacity. Creates the sense of an "AI brain" pulsing behind the interface.
- **Decorative Watermarks:** Oversized Material Symbols at 20% opacity, positioned absolutely as background texture within glass cards.
- **Giant Background Text:** Headline font at `text-[20vw]`, ~2% opacity, anchored to section bottoms as subliminal branding.
- **Scroll Indicator:** Centered gradient line fading from `primary` to transparent, with uppercase micro-label above.
- **Intentional Asymmetry:** Offset columns and asymmetric grid splits create a modern editorial rhythm. Avoid perfectly balanced layouts.

### Responsive Behavior

- Desktop-first design (project target: `DESKTOP`, 2560px canvas).
- Navigation links hidden on mobile (`hidden md:flex`).
- Grid collapses from multi-column to single-column below `md` breakpoint.
- Typography scales down from hero sizes on smaller viewports (`text-6xl md:text-8xl`).

---

## 6. Do's and Don'ts

### Do
- Use intentional asymmetry and editorial rhythm in layouts
- Use the Spacing Scale rigorously — precision margins define premium design
- Use "Teal Glows" (large radial gradients of `primary` at 5% opacity) in far backgrounds
- Use the Signature Curve on all interactive and container elements
- Define boundaries through background color tier shifts
- Use glassmorphism for floating/overlay elements
- Keep text at `on-surface` (`#dfe3e1`), never pure white on black

### Don't
- Use standard blue/grey Material Design shadows
- Use 100% white text — always use the muted `on-surface` token
- Use 1px solid borders to section content
- Use 100% opaque, high-contrast borders
- Use perfectly circular buttons — lean into the Signature Curve
- Crowd the interface — increase padding with `16`/`20` spacing tokens if it feels busy
- Use horizontal divider lines between list items or cards
