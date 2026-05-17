---
name: supanova-redesign-engine
description: Upgrades existing Next.js landing pages to premium quality. Audits current design for generic AI patterns and applies Supanova's high-end standards on Next.js App Router + Tailwind CSS v4 + shadcn/ui. Works with any Next.js codebase — App Router or Pages Router, Tailwind v3 or v4, with or without shadcn.
---

# Supanova Redesign Engine

## How This Works

When applied to an existing Next.js landing page, follow this sequence:

1. **Scan** — Read `app/` (or `pages/`), `globals.css`, `tailwind.config.*`, `next.config.*`, `components/`. Identify:
   - App Router vs Pages Router
   - Tailwind v3 (config file) vs v4 (`@import "tailwindcss"` + `@theme`)
   - Font loading: `next/font` vs `<link>` vs none
   - Component primitives: shadcn, MUI, Mantine, Chakra, raw Tailwind
   - Icon library: lucide, heroicons, react-icons, iconify
   - Image strategy: `next/image` vs raw `<img>`
   - Color tokens, current palette, layout patterns
2. **Diagnose** — Run through audit below. Document every generic pattern, weak point, missing element.
3. **Fix** — Apply targeted upgrades. Do not rewrite from scratch. Improve in place while maintaining existing structure and routing.

## Stack Migration Triggers

Upgrade the foundation when these red flags appear:

- **Tailwind CDN (`<script src="https://cdn.tailwindcss.com">`) in `app/layout.tsx`** — Replace with proper PostCSS Tailwind v4 install.
- **Tailwind v3 with verbose `theme.extend` for things v4 handles natively** — Migrate to `@theme` block in `globals.css`. Run `npx @tailwindcss/upgrade` if scope allows.
- **Inline `<style jsx>` blocks for color tokens** — Move to `@theme` CSS variables.
- **Web component `<iconify-icon>` in JSX** — Replace with `@iconify/react` `<Icon />`. The web component is brittle in React (registration order, SSR mismatch).
- **Raw `<img src="https://picsum.photos/...">`** — Migrate to `next/image` + `next.config.ts` `remotePatterns`.
- **`font-family` hardcoded as `"Inter"` or `"Noto Sans KR"` via `<link>`** — Migrate to `next/font/local` Pretendard + `@theme --font-sans` token.
- **`"use client"` at the top of `app/page.tsx`** — Push the boundary down. Identify the actually-interactive subtree, extract it, leave the page as a Server Component.
- **`pages/index.tsx` with no App Router migration on the table** — Note it but respect user's choice. Apply visual upgrades in the existing router.

## Design Audit for Landing Pages

### Typography

- **Browser default, Inter, or Noto Sans KR.** Replace with Pretendard (Korean standard) loaded via `next/font/local` + premium English display font (Geist via `next/font/google`, or Outfit/Cabinet Grotesk/Satoshi). Wire through `@theme --font-sans` token.
- **Headlines lack presence.** Korean headlines need `text-4xl md:text-6xl tracking-tight leading-tight font-bold`.
- **Missing `word-break: keep-all` on Korean text.** Add globally in `globals.css` `@layer base` on `body`, or per-block via `break-keep`.
- **Body text too wide.** Constrain to ~65 characters (`max-w-[65ch]`). Increase `line-height` for Korean readability.
- **Only Regular and Bold weights.** Pretendard is a variable font — request weight `500` and `600` for hierarchy depth.
- **Numbers in proportional font.** Use `tabular-nums` Tailwind utility for metrics and pricing.
- **Orphaned words.** Fix with `text-wrap: balance` (Tailwind `text-balance`) on headings.

### Color and Surfaces

- **Pure `#000000` or `#ffffff` background.** Replace with slightly tinted neutrals in `@theme` palette tokens.
- **Oversaturated accents.** Keep saturation < 80%. Express palette in OKLCH inside `@theme`.
- **Multiple accent colors competing.** Pick ONE. Remove the rest from tokens AND from JSX.
- **Purple/blue "AI gradient" aesthetic.** Most common AI design fingerprint. Replace with neutral base + single considered accent.
- **Generic `box-shadow`.** Tint shadows to background hue. Add bespoke shadow tokens (`--shadow-elev-1`, `--shadow-elev-2`) in `@theme`.
- **Flat design with zero texture.** Add subtle noise overlay (one fixed div in `layout.tsx`), mesh gradient background, or micro-patterns.
- **Random dark section in a light page.** Maintain consistent background tone. Use shade variations, not dramatic jumps.

### Layout (Next.js / Landing Page Specific)

- **Everything centered and symmetrical.** Break symmetry with offset margins, mixed aspect ratios, split-screen layouts.
- **Three equal card columns for features.** Most generic AI layout. Replace with Bento grid, zig-zag, or horizontal scroll.
- **Every section uses the same layout.** Adjacent sections MUST use different patterns. Hero (split) → Features (bento) → Testimonials (masonry) → CTA (full-bleed).
- **`height: 100vh` / `h-screen`.** Replace with `min-h-[100dvh]` for iOS Safari compatibility.
- **No max-width container.** Add `max-w-7xl mx-auto px-4 sm:px-6 lg:px-8`.
- **Missing whitespace.** Double section padding. `py-20 md:py-32` minimum.
- **Cards of forced equal height.** Allow variable heights or use masonry.
- **No overlap or depth.** Use negative margins, z-index layering, or overlapping elements for visual depth.
- **CTA buttons not prominent enough.** Need `px-8 py-4 text-lg` minimum. If shadcn `<Button>` is in use, add a `supanova` variant in `components/ui/button.tsx`.
- **All sections inside one giant `page.tsx`.** Extract each section into `components/sections/<name>.tsx`. Keep `page.tsx` as a thin composition shell.

### Interactivity and States

- **No hover states on buttons.** Add `hover:scale-[1.02]`, background shift, or translate effect with smooth transition.
- **No active feedback.** Add `active:scale-[0.98]`.
- **Instant transitions.** Add `transition-all duration-300 ease-[cubic-bezier(0.16,1,0.3,1)]` to all interactive elements. Expose as `--ease-supanova` in `@theme`.
- **No scroll animations.** Add fade-up reveals using `motion/react` `useInView` + `<motion.div whileInView=...>`. Section component becomes `"use client"`.
- **No loading states.** Add skeleton shimmer or Suspense boundaries.
- **Static logo strips.** Convert to auto-scrolling CSS marquee for trust logos.
- **Dead `href="#"`.** Remove or visually disable.
- **No smooth scroll.** Add `scroll-behavior: smooth` to `html` in `@layer base`.
- **`window.addEventListener('scroll')` for scroll-driven effects.** Replace with `useScroll`/`useInView` from `motion/react`.

### Korean Content Quality

- **Translated-sounding Korean.** Rewrite in native, natural Korean. "지금 시작하세요" not "시작을 하세요 지금".
- **Mixed honorifics.** Stick to 합니다/하세요 consistently.
- **AI cliches.** Remove: "혁신적인", "원활한", "차세대", "게임 체인저", "한 차원 높은". Use concrete language.
- **Generic placeholder names.** Replace "김철수", "이영희" with: "하윤서", "박도현", "이서진".
- **Fake round metrics.** Replace `50,000+` with `47,200+`. `5.0/5.0` with `4.87/5.0`.
- **English placeholder text.** All visible content in Korean.
- **Lorem Ipsum.** Replace with real Korean draft copy immediately.

### Component Patterns (Landing Page)

- **Generic centered hero over solid color.** Split screen, full-bleed `<Image fill />`, or asymmetric statement layout.
- **3-card feature row.** Replace with Bento grid, zig-zag, or horizontal scroll strip.
- **Carousel testimonials with dots.** Replace with masonry wall, embedded social-style cards, or single rotating quote with large portrait.
- **Pricing table with 3 identical towers.** Highlight recommended tier with color, scale, and emphasis. Use shadcn `<Card>` with a custom `featured` prop.
- **Footer link farm with 4+ columns.** Simplify to essential nav, legal, social.
- **Accordion FAQ.** Replace with side-by-side list, searchable help, or expandable inline sections. shadcn `<Accordion>` is fine if styled away from the default.
- **CTA that blends in.** Need dramatic visual contrast — different background, larger padding, floating treatment.

### Icons and Images

- **Lucide or Feather everywhere.** Replace with `@iconify/react` Solar set for consistency. Keep lucide for shadcn primitives (Check, ChevronDown, etc.) — that's idiomatic.
- **Broken Unsplash URLs.** Replace with `picsum.photos/seed/{name}/{w}/{h}` for landscapes, `i.pravatar.cc/150?u={name}` for avatars. Configure `next.config.ts` `images.remotePatterns`.
- **Raw `<img>` tags.** Migrate to `next/image` with `width`, `height` (or `fill`), `alt`.
- **Missing favicon.** Add `app/favicon.ico` or `app/icon.tsx` for dynamic icon generation.
- **Inconsistent icon stroke weights.** Standardize to Solar set (uniform weight).
- **Generic stock "team" photos.** Use consistent illustration style or high-quality contextual photography.

### Code Quality

- **Div soup.** Use semantic HTML: `<nav>`, `<main>`, `<section>`, `<article>`, `<footer>`. JSX accepts all of them.
- **Missing metadata.** Add `export const metadata: Metadata = { title, description, openGraph, ... }` in `layout.tsx` / `page.tsx`.
- **No `lang="ko"` on `<html>`.** Add it in `app/layout.tsx`.
- **`<img>` without `loading="lazy"`.** Use `next/image` — handled automatically.
- **No `alt` text.** Add descriptive Korean alt.
- **Arbitrary z-index values.** Establish: nav (40), overlay (50), decorative (60).
- **`"use client"` at top of every component.** Audit: keep server components by default; mark client only when the file uses hooks, refs, browser APIs, or motion.
- **`any` types in props.** Replace with explicit interfaces.

## Upgrade Techniques

### Typography Upgrades
- **Animated text reveals.** Characters/words fade/slide in sequentially on scroll via `motion/react` `staggerChildren`.
- **Gradient text accent.** ONE key headline with subtle gradient fill (max one per page).
- **Variable weight on hover.** Text weight shifts subtly when interactive elements hovered — uses Pretendard's variable axis.

### Layout Upgrades
- **Broken grid / asymmetry.** Elements deliberately offset from column structure.
- **Parallax depth.** Background images scroll at different speeds — `useScroll` + `useTransform` from `motion/react`.
- **Sticky scroll stacking.** Sections stick and layer over each other during scroll — `position: sticky` + scroll-linked opacity.
- **Full-bleed section transitions.** Sections bleed into each other with gradient or diagonal transitions.

### Motion Upgrades
- **Staggered entry cascades.** `<motion.div initial={{opacity:0, y:32}} whileInView={{opacity:1, y:0}} viewport={{once:true}} transition={{delay: i*0.08, ease:[0.16,1,0.3,1]}}>`.
- **Spring-based hover.** `transition: { type: "spring", stiffness: 260, damping: 22 }` via `motion/react`.
- **Scroll-driven progress.** SVG line drawings tied to scroll position via `useScroll`.
- **Marquee logos.** CSS `@keyframes marquee` + duplicated row, no JS.

### Surface Upgrades
- **True glassmorphism.** `backdrop-blur-xl` + `border border-white/10` + inner shadow.
- **Mesh gradient backgrounds.** Multiple `radial-gradient` layers in a dedicated `<MeshBackground />` component.
- **Noise texture overlay.** Single fixed `pointer-events-none` div in `layout.tsx`.
- **Tinted shadows.** Shadows carrying background hue, declared as `@theme` tokens.

## Fix Priority

Apply in this order for maximum visual impact, minimum risk:

1. **Stack health** — Kill Tailwind CDN script, kill `<iconify-icon>` web component, kill raw `<img>` on `picsum.photos`. The page should still build first.
2. **Font swap to Pretendard** via `next/font/local` and `@theme --font-sans` — instant premium feel for Korean.
3. **Color palette cleanup** — remove AI purple, desaturate accents, move tokens into `@theme`.
4. **Korean content rewrite** — natural copy, real names, organic numbers.
5. **Hover and active states** — make interface feel alive.
6. **Section extraction** — split monolithic `page.tsx` into `components/sections/*`.
7. **Layout diversification** — break same-section repetition.
8. **Section animation** — staggered reveals via `motion/react`, `useInView` scroll triggers.
9. **Polish spacing and typography** — premium final touch.

## Rules

- Do not break existing page structure or routing. Improve incrementally.
- Output stays inside the existing Next.js project layout. Do not bolt on a separate HTML file.
- Before adding any dependency, check `package.json` first. Don't install `framer-motion` if `motion` is already there; don't install `lucide-react` AND `@iconify/react` if only one is needed.
- Keep changes focused and reviewable. Targeted improvements over total rewrites.
- All content modifications must maintain natural Korean quality.
- Preserve user-authored TypeScript types and prop interfaces. If extending, extend — don't replace.
