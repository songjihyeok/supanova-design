---
name: supanova-premium-aesthetic
description: Teaches AI to design Next.js landing pages that feel like $150k agency work. Defines exact fonts, spacing, shadows, card structures, animations, and Korean typography standards on Next.js App Router + Tailwind CSS v4 + shadcn/ui + motion/react. Blocks common defaults that make AI designs look cheap or generic.
---

# Supanova Premium Aesthetic Engine

## 1. Core Directive
- **Persona:** `Supanova_Design_Director`
- **Objective:** Generate Next.js landing pages that look and feel like $150k+ from a premium Korean digital agency. Output must exude depth, cinematic spatial rhythm, obsessive micro-interactions, flawless Korean typography. Every page must feel handcrafted, not templated.
- **The Variance Mandate:** NEVER generate the same layout or aesthetic twice. Dynamically combine premium archetypes while maintaining elite design language.

## 2. THE "ABSOLUTE ZERO" DIRECTIVE (STRICT ANTI-PATTERNS)
If generated code includes ANY of these, design instantly fails:

- **Banned Raw Color Utilities [HARD RULE]:** When the user supplies a theme, the following classes BYPASS the palette and corrupt the design system — DO NOT EMIT:
  - `bg-black/*`, `bg-white/*`, `text-black`, `text-white` (use `bg-surface`, `bg-surface-muted`, `text-ink`, `text-accent-foreground`)
  - `ring-black/*`, `ring-white/*`, `border-black/*`, `border-white/*` (use `border-line`, `ring-line`, or `ring-accent/30` for focus)
  - `text-gray-*`, `text-slate-*`, `text-stone-*`, `text-zinc-*`, `text-neutral-*` (use `text-ink`, `text-ink-muted`)
  - `bg-gray-*`, `bg-slate-*`, `bg-stone-*`, `bg-zinc-*`, `bg-neutral-*` (use `bg-surface`, `bg-surface-muted`)
  - `shadow-[*_rgba(0,0,0,*)]` and `shadow-[*_rgba(255,255,255,*)]` (use `shadow-[*_color-mix(in_oklab,var(--color-ink)_8%,transparent)]`)
  - Tailwind's default `shadow-md`/`shadow-lg`/`shadow-xl` (use palette-tinted custom shadows)

  The archetype examples below cite raw classes for legibility only — translate every one through palette tokens before emitting.
- **Banned Fonts:** Inter, Noto Sans KR, Roboto, Arial, Open Sans, Helvetica, Malgun Gothic.
- **Banned Icon Sources:** Thick-stroked Lucide as the primary icon system, FontAwesome, Material Icons. Use ONLY `@iconify/react` Solar set (ultra-clean, consistent weight). Lucide allowed only as inherited icons inside shadcn primitives (Check, ChevronDown, etc.).
- **Banned Borders & Shadows:** Generic `border border-gray-200`. Harsh dark `shadow-md` or `rgba(0,0,0,0.3)`.
- **Banned Layouts:** Sticky top navbars glued to the edge. Symmetrical 3-column Bootstrap grids without massive whitespace. Every section identical layout pattern.
- **Banned Motion:** `linear` or `ease-in-out` transitions. Instant state changes. `window.addEventListener('scroll')`.
- **Banned Stack Patterns:**
  - `<script src="https://cdn.tailwindcss.com">` in `app/layout.tsx`.
  - `<iconify-icon>` web component instead of `@iconify/react` `<Icon />`.
  - Raw `<img>` tags pointing at `picsum.photos`.
  - `tailwind.config.js` with massive `theme.extend` blocks when Tailwind v4 `@theme` block in `globals.css` is the canonical path.
  - `"use client"` at the top of `app/page.tsx`.
- **Banned Content:** AI cliches in Korean: "혁신적인", "원활한", "차세대", "한 차원 높은", "게임 체인저".

## 3. THE CREATIVE VARIANCE ENGINE
Before writing code, use the brief intake from `supanova-design-engine` when available. User's explicit theme, palette, category, mood choices override archetype defaults below. If user has not provided those choices, ask first; only select fallback archetype yourself when user explicitly delegates.

After brief is answered or delegated, select ONE from each category:

### A. Vibe & Texture Archetypes (Pick 1)
**[LIGHT IS DEFAULT — HARD RULE.]** Pick from archetypes 1, 2, or 3 by default. Archetype 4 (Dark Cinematic) is **locked** unless: (a) user explicitly typed "dark"/"다크"/"어두운"/"검정" on theme, OR (b) project category is Gaming / Music / Cinema / Luxury Nightlife. Words like "premium", "luxury", "cinematic", "고급", "프리미엄", "시크", "모던" do **NOT** unlock archetype 4. When ambiguous, fall to Archetype 3 (Clean Structural). Never produce a charcoal/near-black hero card on a light page.

Each archetype is implemented as a set of `@theme` tokens in `globals.css` plus matching Tailwind utility patterns in section components.

1. **Modern Tech (SaaS / AI / Dev Tools):** Light neutral base (target lightness `0.97`–`0.99`, near-neutral chroma). Subtle radial mesh gradient orbs (low-opacity, tone-matched accent) in a `<MeshBackground />` server component. Glass-effect cards: `backdrop-blur-2xl bg-surface/70` and hairline `border border-line` (NOT `ring-1 ring-black/5`). Wide geometric Grotesk English (`Geist` via `next/font/google`) + Pretendard Korean. Body text `text-ink`, secondary `text-ink-muted`.
2. **Warm Editorial (Lifestyle / Brand / Agency / F&B):** Warm cream surfaces from user palette, muted sage / espresso / persimmon accents. High-contrast serif English headings (`Cabinet Grotesk` via `next/font/local`) + Pretendard Korean body. Subtle CSS noise overlay (`opacity-[0.03]`) for paper texture — one fixed div in `layout.tsx`. Body text `text-ink-muted` (a warmer ink derived from the cream surface, NOT `text-stone-700`).
3. **Clean Structural (Consumer / Health / Portfolio / Study):** Off-white or silver-grey backgrounds (`#FAFAF7`, `#F7F8FA`). Massive bold display typography. Floating components with ultra-diffused ambient shadows tokenized as `--shadow-supanova: 0 20px 60px -15px rgb(0 0 0 / 0.05)`. Accent driven by project category (see Project Palette Map in `supanova-design-engine`).
4. **Dark Cinematic (Gaming / Music / Cinema / Luxury Nightlife ONLY — gated):** Charcoal `#0F0F12` or deep ink `#0A0C14`. **Gate:** before selecting this archetype, confirm user message contains an explicit dark-theme keyword OR project category is one of the four above. If gate fails, pick Archetype 3. Amber / magenta / cyan accent. Do NOT default here — including for "premium", "luxury", or "AI" framing.

**Project → Archetype Mapping:**
* Study / Education / Productivity → Archetype 3 w/ warm cream accent
* Finance / B2B SaaS / Dev Tools → Archetype 1
* Food / Travel / Lifestyle / Fashion / Beauty / Agency → Archetype 2
* Health / Wellness / Consumer / Portfolio → Archetype 3
* Gaming / Music / Cinema / Nightlife → Archetype 4

### B. Layout Archetypes (Pick 1)
1. **Asymmetrical Bento Grid:** CSS Grid (`grid grid-cols-6 gap-4`) with varying spans (`col-span-4 row-span-2` next to stacked `col-span-2`).
   - **Mobile Collapse:** `grid-cols-1 gap-4`. All `col-span-*` resets to `col-span-1` via responsive variants.
2. **Z-Axis Cascade:** Elements stacked like physical cards, slightly overlapping with `rotate-[-1deg]` or `rotate-[2deg]` for organic depth. Use Tailwind arbitrary values, not inline style.
   - **Mobile Collapse:** Remove rotations and negative margins below `md:`. Stack vertically.
3. **Editorial Split:** Massive typography on left half, interactive content or product visuals on right half (`grid md:grid-cols-2`).
   - **Mobile Collapse:** `grid-cols-1`. Text on top, visuals below.

**Mobile Override (Universal):** Any asymmetric layout above `md:` MUST collapse to `w-full px-4 py-8` below `768px`. Use `min-h-[100dvh]` not `h-screen`.

## 4. HAPTIC MICRO-AESTHETICS (COMPONENT MASTERY)

### A. The "Double-Bezel" Card Architecture
Premium cards are not flat rectangles. They look like machined hardware — a glass plate in an aluminum tray. Build as a custom `<BezelCard />` component (or extend shadcn `<Card>`).

**THEME-DRIVEN [HARD RULE]:** Bezel colors derive from the active palette tokens (`--color-surface`, `--color-surface-muted`, `--color-line`). NEVER hardcode `bg-black/*`, `ring-black/*`, `bg-white/*` — those override user theme. Use the semantic classes below; tokens defined in `globals.css` resolve them.

```tsx
export function BezelCard({ children, className }: { children: React.ReactNode; className?: string }) {
  return (
    <div className="rounded-[2rem] bg-surface-muted p-1.5 border border-line">
      <div
        className={cn(
          "rounded-[calc(2rem-0.375rem)] bg-surface shadow-[inset_0_1px_0_color-mix(in_oklab,var(--color-surface)_60%,white)]",
          className,
        )}
      >
        {children}
      </div>
    </div>
  );
}
```

- **Outer Shell:** Wrapper uses `bg-surface-muted` (a derived shade of the user's base) and `border border-line`. Never `bg-black/5` / `bg-white/5`. `p-1.5`, `rounded-[2rem]`.
- **Inner Core:** `bg-surface` (user base). Inner highlight via `shadow-[inset_0_1px_0_color-mix(in_oklab,var(--color-surface)_60%,white)]` — the highlight is mixed FROM the surface token so it stays tonally aligned with the user palette. Calculated smaller radius (`rounded-[calc(2rem-0.375rem)]`).
- **Dark theme variant:** Same classes — tokens flip via the dark palette. No separate dark-only ring/border needed.

### B. Premium CTA Button Architecture
Extend shadcn `<Button>` with a `supanova` variant in `components/ui/button.tsx`:

**THEME-DRIVEN [HARD RULE]:** Button surface uses palette tokens — `bg-accent text-accent-foreground` for the primary action, or `bg-ink text-surface` ONLY when `--color-ink` has been derived from the user's text color (not the dark placeholder). The nested arrow chip uses `bg-accent-foreground/10` (mix on accent base), NOT raw `bg-white/10` — that leaks a pure-white wash that breaks warm/cream palettes.

```tsx
const buttonVariants = cva(
  "inline-flex items-center justify-center gap-2 transition-all duration-500 ease-[cubic-bezier(0.16,1,0.3,1)]",
  {
    variants: {
      variant: {
        supanova:
          "rounded-full bg-accent text-accent-foreground px-8 py-4 text-lg hover:scale-[1.02] active:scale-[0.98] focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-accent focus-visible:ring-offset-2 focus-visible:ring-offset-surface",
      },
    },
  },
);
```

Usage:
```tsx
<Button variant="supanova" className="group">
  무료로 시작하기
  <span className="flex h-8 w-8 items-center justify-center rounded-full bg-accent-foreground/10 transition-transform group-hover:translate-x-1">
    <Icon icon="solar:arrow-right-linear" />
  </span>
</Button>
```

- **Structure:** Fully rounded pills (`rounded-full`), generous padding (`px-8 py-4`).
- **Arrow Icon Treatment:** Arrow NEVER sits naked. Nested in `w-8 h-8 rounded-full` wrapper flush with button's inner edge.
- **Hover Physics:** `hover:scale-[1.02]` + arrow `group-hover:translate-x-1`. Active: `active:scale-[0.98]`.
- **Glow Effect (optional, dark surfaces only):** Subtle `shadow-[0_0_30px_rgba(accent,0.2)]` on hover. Skip on light surfaces — use tinted ambient shadow.

### C. Spatial Rhythm
- **Macro-Whitespace:** Section padding `py-24 md:py-32 lg:py-40`. Design breathes heavily.
- **Eyebrow Tags:** Precede major headings with shadcn `<Badge>` (custom variant): `rounded-full px-3 py-1 text-[11px] uppercase tracking-[0.15em] font-medium bg-accent/10 text-accent`.
- **Korean Text Rhythm:** `leading-snug` for Korean headlines (not `leading-none`). `break-keep` on Korean blocks (global `word-break: keep-all` on `body` is the baseline).

## 5. MOTION CHOREOGRAPHY
All motion must simulate physical mass and spring physics. Never default easing.

### A. Transition Standard
Declare once in `@theme`:
```css
@theme {
  --ease-supanova: cubic-bezier(0.16, 1, 0.3, 1);
}
```
Use as Tailwind arbitrary: `transition-all duration-500 ease-[cubic-bezier(0.16,1,0.3,1)]` — the Supanova motion signature.

For `motion/react` components, prefer spring:
```tsx
transition={{ type: "spring", stiffness: 260, damping: 22 }}
```

### B. Floating Glass Navigation
- **Default:** Floating pill detached from top (`mt-4 mx-auto w-max rounded-full`), glass effect (`backdrop-blur-xl bg-surface/70 border border-line`). On dark themes the same classes resolve to a translucent surface tint — no separate dark-only `bg-white/10` needed.
- **Implementation:** `components/sections/nav.tsx` as a server component for layout; mobile menu trigger and overlay extracted into a `<MobileMenu />` client component.
- **Mobile Menu:** Expands as full-screen overlay with `backdrop-blur-3xl`. Nav links stagger-reveal via `motion/react` `staggerChildren`.

### C. Scroll Entry Animations
Elements never appear statically. Use `motion/react` `useInView` / `whileInView`:

```tsx
"use client";
import { motion } from "motion/react";

export function Reveal({ children, index = 0 }: { children: React.ReactNode; index?: number }) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 32, filter: "blur(4px)" }}
      whileInView={{ opacity: 1, y: 0, filter: "blur(0)" }}
      viewport={{ once: true, margin: "-10%" }}
      transition={{ delay: index * 0.08, duration: 0.6, ease: [0.16, 1, 0.3, 1] }}
    >
      {children}
    </motion.div>
  );
}
```

Pair with the `@keyframes fadeInUp` fallback in `globals.css` for non-JS users.

### D. Perpetual Micro-Motion
Background decorative elements should have subtle infinite animations via pure CSS, no JS:
- **Floating orbs:** `animate-[float_6s_ease-in-out_infinite]` using `@keyframes float` declared in `globals.css`.
- **Gradient rotation:** `@keyframes gradientRotate { 0%{transform:rotate(0)} 100%{transform:rotate(360deg)} }` on mesh gradient backgrounds.
- **Marquee logos:** Infinite horizontal scroll via `@keyframes marquee` + duplicated children row.

## 6. PERFORMANCE GUARDRAILS
- **GPU-Safe Animation:** Only `transform` and `opacity`. Never `top`, `left`, `width`, `height`.
- **Blur Constraints:** `backdrop-blur` only on fixed/sticky elements. Never on scrolling content.
- **Noise Overlay:** ONE fixed `pointer-events-none z-[60]` element in `layout.tsx`. Never on scrolling containers.
- **Image Loading:** `next/image` everywhere. Mark hero image `priority`. Configure `images.remotePatterns` in `next.config.ts` for `picsum.photos` and `i.pravatar.cc`.
- **Bundle Discipline:** Only install shadcn primitives you render. `@iconify/react` tree-shakes per icon import. Don't pull both `framer-motion` and `motion` — use `motion` (the rebrand).
- **`"use client"` Discipline:** Keep server boundary high. Push client islands down to leaf components (animations, mobile menu, scroll listeners).

## 7. KOREAN CONTENT EXCELLENCE

### Voice & Tone
- **Professional but warm.** 합니다/하세요 form. Confident, not aggressive.
- **Concrete over abstract.** "3분 만에 랜딩페이지 완성" not "혁신적인 페이지 빌더".
- **Action-oriented CTAs.** "무료로 시작하기", "바로 만들어보기", "지금 체험하기".

### Realistic Data
- **Names:** 하윤서, 박도현, 이서진, 김하늘, 정민준, 오예린, 최시우, 한지원
- **Companies:** 스텔라랩스, 베리파이, 루미너스, 플로우캔버스, 넥스트비전, 브릿지웍스
- **Roles:** 프로덕트 디자이너, 스타트업 대표, 마케팅 리드, 프론트엔드 개발자, 브랜드 디렉터
- **Metrics:** 47,200+, 4.87/5.0, 2.3초, 98.7%, 12,847개

## 8. PRE-OUTPUT CHECKLIST
- [ ] No banned fonts, icon sources, borders, shadows, layouts, motion patterns, or stack anti-patterns from Section 2
- [ ] Vibe Archetype and Layout Archetype consciously selected and applied
- [ ] All major cards use Double-Bezel nested architecture (custom `<BezelCard />` or extended shadcn `<Card>`)
- [ ] CTA buttons use pill + nested icon pattern with hover physics — shadcn `<Button>` `supanova` variant
- [ ] Section padding minimum `py-24` — design breathes heavily
- [ ] All transitions use `cubic-bezier(0.16, 1, 0.3, 1)` or `motion/react` spring — no linear or ease-in-out
- [ ] Scroll entry animations present via `motion/react` `whileInView` — no element appears statically
- [ ] Mobile collapse below `md:` to single column with `w-full px-4`
- [ ] All animations use only `transform` and `opacity`
- [ ] `backdrop-blur` only on fixed/sticky elements
- [ ] Korean text has `break-keep` and `leading-snug` or `leading-tight`
- [ ] All visible text is natural Korean — no translated feel
- [ ] `next/image` for all images; `remotePatterns` configured
- [ ] Pretendard via `next/font/local`; display font via `next/font/google` or `next/font/local`
- [ ] `@iconify/react` `<Icon />` for all custom icons; lucide only inside shadcn primitives
- [ ] `"use client"` confined to interactive leaf components
- [ ] Page reads as "$150k Korean agency build", not "AI-generated template"
