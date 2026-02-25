# GTM Consulting Site — System Rules

## 1. What This Is
- Personal consulting landing page for Lucas Linares — GTM Engineering consulting.
- Single-page site: `index.html`. No framework, no build step.
- Stack: HTML + Tailwind CSS (CDN) + vanilla JS.

## 2. Design System
- **Fonts**: Syne (display/headings), Outfit (body)
- **Color tokens** (defined in `tailwind.config`):
  - `brand-bg`: `#080a11` (page background)
  - `brand-surface`: `#0f1219` (cards)
  - `brand-accent`: `#00e5bf` (teal — CTAs, highlights, accent text)
  - `brand-text`: `#e8eaf0` (primary text)
  - `brand-muted`: `#858ea3` (secondary text)
  - `brand-border`: `#1c2133` (card/section borders)
- **Visual effects**: Dot grid background, film grain overlay, ambient glow orbs, scroll-reveal animations.
- **Card pattern**: `bg-brand-surface border border-brand-border rounded-xl p-8 card-glow`
- **CTA buttons**: `.btn-glow` class. Primary = solid accent bg. Secondary = border with hover accent.
- Keep the dark, minimal, high-contrast aesthetic. No color additions outside the existing palette.

## 3. Core Behavior
- Tone: Direct, concise. Match the site's existing copy voice — punchy, confident, no fluff.
- When editing copy, preserve the headline rhythm (short lines, line breaks for emphasis).
- When adding sections, follow the existing pattern: `reveal` class on containers, `stagger` on grids, horizontal `rule-accent` dividers between sections.

## 4. Research Folder
- `research/` contains competitive analysis, buyer intelligence, positioning docs, and design specs.
- These are reference material — read them for context but don't modify unless asked.

## 5. Editing Rules
- All changes happen in `index.html` unless creating new pages.
- Preserve inline Tailwind config and styles in `<head>` — don't extract to separate files unless asked.
- Test changes by opening the file directly in browser (no server needed).
- Keep the page fast — no additional JS libraries or frameworks without asking.
