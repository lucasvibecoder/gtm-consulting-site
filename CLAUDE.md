# Thresh (runthresh.com) — System Rules

## 1. What This Is
- Landing page for Thresh — signal-based outbound for niche B2B companies.
- Single-page site: `index.html`. No framework, no build step.
- Stack: HTML + Tailwind CSS (CDN) + vanilla JS.
- Live at runthresh.com, deployed via Vercel from `main` branch.

## 2. Design System
- **Fonts**: Syne (display/headings), Outfit (body), JetBrains Mono (code/email cards)
- **Color tokens** (defined in `tailwind.config`):
  - `brand-bg`: `#0A0A0A` (page background)
  - `brand-surface`: `#141414` (cards)
  - `brand-surface-alt`: `#1A1A1A` (card variants)
  - `brand-border`: `#242424` (card/section borders)
  - `brand-accent`: `#D4622B` (orange — CTAs, highlights, accent text)
  - `brand-accent-hover`: `#E87040` (hover state)
  - `brand-accent-light`: `rgba(212, 98, 43, 0.10)` (subtle accent bg)
  - `brand-text`: `#E8E6E1` (primary text)
  - `brand-muted`: `#8A8880` (secondary text)
  - `brand-faint`: `#4A4A45` (tertiary/disabled text)
- **Card pattern**: `bg-brand-surface border border-brand-border rounded-xl p-8 card-lift`
- **CTA buttons**: `.btn-primary` class. Primary = solid accent bg. Secondary = border with hover fill.
- Keep the dark, minimal, high-contrast aesthetic. No color additions outside the existing palette.

## 3. Core Behavior
- Tone: Direct, concise. Match the site's existing copy voice — punchy, confident, no fluff.
- When editing copy, preserve the headline rhythm (short lines, line breaks for emphasis).
- When adding sections, follow the existing pattern: `reveal` class on containers, `stagger` on grids, `.divider` between sections.

## 4. Research Folder
- `research/` contains competitive analysis, buyer intelligence, positioning docs, and design specs.
- These are reference material — read them for context but don't modify unless asked.

## 5. Editing Rules
- All changes happen in `index.html` unless creating new pages.
- Preserve inline Tailwind config and styles in `<head>` — don't extract to separate files unless asked.
- Test changes by opening the file directly in browser (no server needed).
- Keep the page fast — no additional JS libraries or frameworks without asking.

## 6. SEO
- Meta description, Open Graph, and Twitter card tags are in `<head>`.
- `robots.txt` and `sitemap.xml` exist at project root.
- Update `sitemap.xml` lastmod date when making content changes.
