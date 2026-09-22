---
name: modern-web-design-system
description: A premium SaaS/agency-style design system distilled from studying a real production site's HTML/CSS markup (fonts, color tokens, glassmorphism, bento grids, entrance animations, accessible components). Use this whenever building or redesigning a landing page, portfolio site, pitch demo, one-page mockup, or marketing site for a client or for the user's own outreach (e.g. a "free premium redesign" sent to a prospect) — even if the user just says "make it look modern/premium" or "build me a landing page" without naming a specific style. Also use it as a checklist when reviewing an existing page for a more polished, conversion-oriented look and feel. Reuse the structure and visual language here, but never copy another site's exact copy, brand name, or logo — headlines and content must be original to whatever project this is applied to.
---

# Modern Web Design System

A pattern library distilled from analyzing the markup of a real, well-built Next.js/Tailwind agency site. It captures *how* a premium-feeling site is actually built — the concrete class combinations, color math, and component shapes — not just vague adjectives like "clean" or "modern." Use it as a toolbox: pull the pieces that fit the project, adapt the colors and copy, skip what doesn't apply.

This is a reusable pattern reference, not a template to clone verbatim. The component *shapes* (a two-path split, a bento grid, an accordion FAQ) are generic and safe to reuse anywhere. Anything that reads as another business's identity — their name, logo, exact headlines, portfolio content — must never appear in someone else's project.

## When this applies

Reach for this system whenever the deliverable is a page meant to persuade — a business site, an eCommerce storefront redesign, a SaaS/agency landing page, a portfolio, a "here's what your store could look like" pitch mockup. It's built framework-agnostic: the class names below are Tailwind, but the underlying moves (color tokens, spacing rhythm, motion rules, component shapes) translate to plain CSS, Shopify Liquid, or any other stack — translate the utility classes to whatever the target styling system uses.

## Stack baseline (adapt to the project's actual stack)

The reference site runs Next.js 15 + React 19 + TypeScript + Tailwind v4, but the *techniques* apply regardless of stack:

- **Fonts**: pair one humanist sans for body/UI (the reference uses Manrope + Inter) with a monospace face (Geist Mono, JetBrains Mono, or similar) reserved for labels, tags, stats, and anything that should feel "technical." Load both as CSS variables so the mono face is a deliberate accent, not the whole page.
- **Icons**: one consistent icon set throughout (lucide-react in the reference) — inline SVG, `stroke-width="2"`, sized via a handful of fixed classes (`size-4`, `size-5`). Never mix icon families.
- **Images**: always wrap in an aspect-ratio container, serve responsive `srcset`/`sizes`, and default to `object-cover` with a deliberate focal point (`object-top` for tall screenshots).
- **SEO baseline**: JSON-LD `Organization` + `WebSite` schema, full OG/Twitter meta tags, a `theme-color` meta for both light and dark, and a canonical link. This is cheap to add and most hand-built landing pages skip it.
- **Optional load screen**: for a heavier app-like build, a full-viewport brand overlay (logo + a thin gradient progress bar animating 0%→100%) that fades out once the page is ready reads as more "product" than "static site." Skip this for a simple marketing page — it's overkill there.

## Color system

The core trick is **one brand color + a small rotating accent palette for category-coding**, laid over strict neutrals — not a rainbow of arbitrary colors.

```
--brand:        #6D35FF   /* primary: CTAs, links, focus rings, gradient stops */
--ink:          #121212   /* near-black text and dark section backgrounds */
--ink-soft:     #0a0a0a   /* darkest end of dark-section gradients */
--body-grey:    #5F5F5F   /* secondary/body text on light backgrounds */
--body-grey-2:  #2c2c2c   /* slightly darker body text variant */

/* Accent rotation — assign one per category/card, never reuse arbitrarily */
--accent-cyan:   #2ebfff  /* AI / tech / data */
--accent-orange: #fd822b  /* automation / infra / speed */
--accent-pink:   #fc4883  /* used sparingly, e.g. in aurora backgrounds */
--accent-violet: #8B5CF6  /* secondary purple, e.g. fintech/trading */
```

Rules that make this read as premium rather than random:

1. **Theme-token indirection.** Don't hardcode grays everywhere — define `--color-heading`, `--color-body`, `--color-surface-soft`, `--color-border-soft` once and reference those. It means a dark-mode or rebrand pass touches one file, not every component.
2. **Tinted shadows, not black ones.** A primary button's shadow should pick up the brand color: `box-shadow: 0 12px 32px -12px rgba(109,53,255,0.7)` reads far more premium than a generic `rgba(0,0,0,0.2)`.
3. **Icon chips = 20% tint of the accent as background, full accent as the icon stroke.** In hex-alpha shorthand: background `#6D35FF20`, icon `color: #6D35FF`. This single trick is most of what makes a row of feature icons look designed instead of default.
4. **Each repeating card in a grid gets a distinct accent from the rotation**, cycling in a fixed order — never randomized per render, so a returning visitor sees consistent color-coding.

## Typography

- **Hero H1**: scale up across breakpoints instead of one fixed size — roughly `31px → 44px → 54px` mobile/tablet/desktop, `font-black`, tight tracking (`letter-spacing: -0.03em` to `-0.045em`), tight leading (`line-height: 1.05–1.06`). Highlight the key phrase in the brand color via an inline `<span>` — don't color the whole heading.
- **Section H2**: `text-4xl` to `text-6xl` depending on section weight, same tight tracking/leading, same colored-span trick.
- **Eyebrow/kicker label above every section heading**: small caps, uppercase, `11–12px`, `font-semibold`, brand color, letterspacing `0.08em` (e.g. "Selected work", "How We Work"). This one element does a lot of work to make sections feel authored rather than dumped on the page.
- **Mono font is a deliberate signal**, not a body font: use it for stat labels, tab labels, tag pills, terminal-style UI chrome, and uppercase tracked-out microcopy. Mixing it in makes the page feel "engineered."
- **Body copy**: `15–16px`, line-height `1.55–1.65`, using the muted grey token — never pure black body text, it reads harsh next to a `font-black` heading.

## Component patterns

Pull whichever of these fit the page being built. Each is described as a *shape*, so it translates to any stack.

1. **Floating glass navbar** — not full-width. A pill-shaped nav sits inside a `max-w-4xl` centered container with top margin, `backdrop-filter: blur(28px) saturate(190%)`, a semi-transparent white gradient background, and a combined inset+drop shadow for a frosted edge. A subtle radial-gradient glow can track cursor position on hover for extra polish.

2. **Hero** — centered content in a `max-w-3xl` column over a soft gradient (`white → brand/5% → white`), with two large blurred color circles (`blur-3xl`) placed absolutely for depth. Primary CTA is a solid brand-color pill that lifts on hover (`hover:-translate-y-0.5`) and darkens; secondary CTA is an outlined pill that inverts (fills solid) on hover. An optional "ask AI what you need" input bar with quick-suggestion pills underneath works well for a product/agency pitch.

3. **Two-path split** — side-by-side cards segmenting visitors early into two buyer types (e.g. "quick fix" vs. "custom build"). Each card: a top accent bar fading to transparent, an icon chip, a small mono-caps route label, a heading, a checkmarked feature list, and two CTAs (a solid dark pill that turns brand-color on hover, plus a plain text link as the lower-commitment option).

4. **Flagship case-study card** — asymmetric grid (roughly 5-col text / 7-col visual on desktop). Wrap the screenshot in a fake browser-chrome frame: three "traffic light" dots + a mock URL bar. If the case study has sub-views, use a real `role="tablist"`/`role="tabpanel"` to switch between them, not custom divs.

5. **Portfolio/product grid cards** ("glass-card") — image on top with a gradient fade at both top and bottom edges, category badge pinned top-left, then title, subtitle, a 2–3 line description, a row of meta tags, and a split footer (case-study link on the left, external "Visit" link on the right). An autoscrolling tall-screenshot animation on hover adds motion without a video.

6. **Dark interactive carousel** — for showing several live demos/products. Near-black container (`#0c1013`), a slide counter (`01 / 10`), circular prev/next buttons, and a distinct accent color per slide. A "reveal interface" progressive-disclosure button that expands a hidden panel (browser-chrome frame + a live status sidebar showing current action/result/progress) turns a static screenshot into something that feels like a real product demo.

7. **Bento capability grid** (dark section) — a 4-column grid of glass-morphic cards. Each card: icon chip + an oversized "ghost" stat or label in the corner, a small abstract animated diagram themed to that card's accent color (built from simple divs/SVG — a fake dashboard, terminal, or graph, not a real screenshot), a title + mono description, and an underline that scales in on hover plus a soft gradient border glow around the whole card on hover.

8. **Infinite marquee strip** — a horizontally auto-scrolling row of pill badges (dot + label) for a tech stack, client logos, or trust badges. Duplicate the content once so the loop is seamless.

9. **Numbered process steps** — reuse the bento card shape from #7, but number them `01–04` instead of a stat, each with its own small abstract diagram (a scanning radar circle for "discovery," an animated checklist for "testing," a launch-trajectory SVG path for "deploy," a live sparkline for "monitor").

10. **Accordion FAQ** — rounded container, divided rows, a plus/minus icon inside a tinted circle, real `aria-expanded` + a height transition (not `display: none`/`block`).

11. **Aurora closing CTA** — a glassmorphic frosted panel floating over 2–3 large blurred color blobs (brand + 1–2 accents) for an "aurora" background effect. A subtle 3D tilt on the panel (`perspective` + `translateZ` on the inner content) adds depth. Gradient-bordered pill CTA + a plain outlined secondary CTA.

12. **Footer** — dark, repeats the closing headline once more, then a 5–6 column link grid (logo + one-line blurb, then link columns for Services/Work/Company/Contact), an optional trust row of third-party profile badges (Google, Clutch, industry directories), and a bottom bar with copyright + a one-line proof statement.

## Motion & interaction rules

- **Entrance animation**: elements start `opacity: 0; transform: translateY(20–30px)` (or `translateX`, or `scale(0.5–0.8)` for numeric/icon reveals, or `scaleX(0)` for underlines/progress bars) and animate to their resting state on scroll into view. Stagger by list position so a grid or list cascades in rather than popping all at once.
- **Hover lift** on primary buttons: `hover:-translate-y-0.5` combined with a color darken and a shadow that grows — three signals reinforcing one interaction, not just a color change.
- **Icon micro-interaction**: arrow icons inside links/buttons shift 2–4px right on hover (`group-hover:translate-x-0.5`) — a small, cheap detail that reads as "polished."
- **Never skip focus states.** Every interactive element gets `focus-visible:ring-2` in the brand color plus a ring offset. This isn't optional decoration — it's what makes the page pass a real accessibility check, and skill-built sites routinely omit it.
- **Use real ARIA roles** for tabs (`role="tablist"`/`role="tabpanel"`) and accordions (`aria-expanded`), not div-soup with onClick handlers standing in for them.
- **Touch targets**: enforce a minimum 44px height (`min-h-11`/`min-h-12`) on anything tappable — buttons, accordion headers, nav links in a mobile menu.

## Copy & structure patterns (for agency/services/portfolio pages specifically)

- **Segment visitors early.** A two-path split near the top routes different buyer types to different proof and different CTAs, instead of making everyone read the same generic pitch.
- **Every section opens with an eyebrow + a punchy two-line heading**, one phrase colored — see Typography above.
- **State proof as mechanism, not adjective.** "Milestone-based delivery. Working software every week. You keep the source code." lands harder than "we're reliable and transparent." Say what actually happens, not how it should make the reader feel.
- **Let the FAQ pre-empt objections** — ownership/IP, payment structure, what happens if requirements change — rather than leaving them for a sales call.
- **Keep multiple low-friction contact paths visible** (chat app, email, book-a-call) in both the hero and the footer, not buried on a separate contact page only.

## Applying this to a Shopify/eCommerce project

Most of this maps directly onto a Shopify theme or a storefront redesign mockup even though the reference site is a Next.js agency page:

- The color-token + accent-rotation system works in a `theme.liquid`/CSS custom-properties setup exactly as described.
- The bento grid, accordion FAQ, and marquee strip are common needs on eCommerce landing pages (features grid, shipping/returns FAQ, "as seen in" logo strip) — reuse the shapes as-is.
- The hero pattern (soft gradient + blurred blobs + dual CTA) works well for a store's homepage hero or for a "premium redesign" mockup sent to a prospect as a free-value pitch.
- Swap "case study" language for "product" language in the portfolio-grid pattern to turn it into a product grid.

## What not to carry over

Never reuse another business's brand name, logo, exact headline copy, client names/logos, or portfolio content — those identify a specific real company. Everything above this line describes *structure and visual language*, which is fair game to reuse; specific words and assets belong to whoever they're actually for.
