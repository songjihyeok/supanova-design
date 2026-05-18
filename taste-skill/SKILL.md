---
name: supanova-design-engine
description: Supanova Landing Page Design Engine. Generates premium, conversion-optimized landing pages on Next.js App Router + Tailwind CSS v4 + shadcn/ui. Overrides default LLM biases toward generic templates. Enforces metric-based design rules, Korean typography standards, and hardware-accelerated motion for production-grade Next.js output.
---

# Supanova Design Engine

## 1. ACTIVE BASELINE CONFIGURATION
* DESIGN_VARIANCE: 8 (1=Perfect Symmetry, 10=Artsy Chaos)
* MOTION_INTENSITY: 6 (1=Static/No movement, 10=Cinematic/Magic Physics)
* VISUAL_DENSITY: 3 (1=Art Gallery/Airy, 10=Pilot Cockpit/Packed Data)
* LANDING_PURPOSE: conversion (conversion | brand | portfolio | saas | ecommerce)
* **SURFACE_MODE: light** (light | dark | ask-first) — Default light. Dark ONLY when user explicitly types "dark mode" / "어두운 테마" / "다크" OR project category is Gaming / Music / Cinema / Luxury Nightlife. Ambiguity, silence, "알아서", "추천", or anything short of explicit dark request → light. Never pick dark to "look cool" or "look premium".
* **BASE_LUMINANCE: high** (high | mid | low) — Light mode surfaces must be genuinely bright. Base `#FAFAF7`–`#FFFFFF` range, never `#0F0F12`. Cards on light base use white or warmer-than-base tints, never dark slabs unless dark mode is active.
* **PROJECT_CATEGORY: ask-first** — Ask user to confirm category before picking palette unless prompt already makes category obvious. See Rule 2 Project Palette Map.

**AI Instruction:** Baseline strictly set to these values (8, 6, 3, conversion). Do not ask user to edit this file. ALWAYS listen to user: adapt these values dynamically based on what they explicitly request. Use these baseline (or user-overridden) values as global variables to drive specific logic in Sections 3 through 8.

**SURFACE DECISION RULE [CRITICAL]:** Light is default. Go dark only when one of these is true: (1) user message contains "dark", "다크", "어두운", "검정", "블랙" applied to theme/배경; (2) project category is Gaming, Music, Cinema, or Luxury Nightlife. Otherwise — including "premium", "luxury", "cinematic", "고급", "프리미엄", "시크" — stay LIGHT. Premium ≠ dark. When in doubt, choose the brightest archetype that fits category.

### REQUIRED DESIGN BRIEF SCRIPT
Before writing code, run this brief intake unless user prompt already answers every item. Ask in Korean by default, keep concise, wait for user answer. Do not silently choose palette, theme, category, motion level, or purpose.

Ask these questions as one message:

1. **목적:** 전환/브랜드/포트폴리오/SaaS/이커머스 중 어떤 목적의 랜딩페이지인가요?
2. **카테고리:** Study, Finance, Health, Food, Dev, Portfolio, Beauty, Entertainment 중 어디에 가장 가깝나요?
3. **테마:** 밝은 테마, 어두운 테마, 시스템/자동 중 무엇을 원하시나요?
4. **분위기:** 미니멀, 프리미엄, 에디토리얼, 테크, 럭셔리, 캐주얼 중 어떤 방향인가요?
5. **컬러:** 원하는 베이스 컬러와 포인트 컬러가 있나요? 없으면 카테고리에 맞춰 2-3개 후보를 제안하세요.
6. **모션:** 정적/은은함/활발함/시네마틱 중 어느 정도가 좋나요?
7. **밀도:** 넓고 여백 많은 화면, 표준 밀도, 정보가 많은 화면 중 무엇을 선호하시나요?
8. **참고:** 참고 사이트, 브랜드, 피해야 할 색상이나 스타일이 있나요?

If user asks for automatic generation, no-question mode, or "알아서", skip wait and proceed with clearly stated recommendation based on project category. If only some answers missing, ask only for missing high-impact choices: theme, palette, purpose.

## 2. DEFAULT ARCHITECTURE & CONVENTIONS

All output is a **Next.js 15 App Router project** designed for production deployment.

### File Layout
```
app/
  layout.tsx          # <html lang="ko">, fonts, metadata, providers
  page.tsx            # composes section components
  globals.css         # Tailwind v4 import + @theme tokens + custom keyframes
components/
  sections/
    nav.tsx
    hero.tsx
    social-proof.tsx
    features.tsx
    testimonials.tsx
    cta.tsx
    footer.tsx
  ui/                 # shadcn/ui components (button, card, badge, ...)
lib/
  utils.ts            # cn() helper from shadcn
public/
  fonts/              # Pretendard self-hosted (optional)
```

### Stack — Non-Negotiable
* **Framework:** Next.js 15 App Router, React 19, TypeScript strict.
* **Styling:** Tailwind CSS v4 via PostCSS plugin (`@tailwindcss/postcss`). NO Tailwind CDN script. NO `tailwind.config.js` content array (v4 is config-less by default). Use `@theme` block inside `app/globals.css` to extend tokens.
* **Component Primitives:** shadcn/ui — install only what is used (`npx shadcn@latest add button card badge separator scroll-area`). Components live in `components/ui/`.
* **Typography — Korean First:**
  * **Primary Font:** `Pretendard` loaded via `next/font/local` from `public/fonts/PretendardVariable.woff2`, exposed as CSS variable `--font-pretendard`. NON-NEGOTIABLE.
  * **English Display Font:** Pair with `Geist`, `Outfit`, `Cabinet Grotesk`, or `Satoshi` via `next/font/google` (Geist/Outfit) or `next/font/local` (Cabinet/Satoshi). Expose as `--font-display`.
  * **Tailwind v4 token:** Map in `@theme` block: `--font-sans: var(--font-pretendard), 'Geist', system-ui, sans-serif;`
* **Icons:** `@iconify/react` with Solar set exclusively. Usage: `import { Icon } from "@iconify/react"; <Icon icon="solar:arrow-right-linear" />`. NO web-component `<iconify-icon>` — it requires extra runtime registration in React.
* **Images:** `next/image` for all images. For placeholders use `https://picsum.photos/seed/{name}/{w}/{h}` (add `picsum.photos` to `next.config.ts` `images.remotePatterns`). For avatars `https://i.pravatar.cc/150?u={name}`. NEVER Unsplash URLs.
* **Animation:** `motion/react` (the React build of Motion) for `MOTION_INTENSITY > 5`. Use `<motion.div>`, `useInView`, `useScroll`. For static or trivial animations use Tailwind `animate-*` utilities and `@keyframes` declared in `globals.css`.
* **ANTI-EMOJI POLICY [CRITICAL]:** NEVER use emojis in JSX or visible text. Replace with `<Icon icon="solar:...">` or shadcn icon primitives.
* **Responsiveness:**
  * Tailwind breakpoints (`sm:`, `md:`, `lg:`, `xl:`).
  * Container: `<div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">`.
  * **Viewport Stability [CRITICAL]:** NEVER use `h-screen`. ALWAYS `min-h-[100dvh]` to prevent iOS Safari layout jump.
  * **Grid over Flex-Math:** Use CSS Grid (`grid grid-cols-1 md:grid-cols-3 gap-6`) over complex flex percentages.
* **Language:** Default content language **Korean**. All placeholder text, headings, descriptions, CTAs in natural professional Korean — not translated.
* **Server vs Client Components:** Default to Server Components. Mark `"use client"` ONLY when a section uses motion hooks, state, refs, IntersectionObserver, or browser-only APIs. Keep client islands as small as possible — extract interactive bits (e.g. `<HeroAnimation />`) and import them into a server-rendered section.

### Required globals.css Skeleton

**USER-THEME-DRIVEN TOKEN GENERATION [HARD RULE].** The values below are **placeholder slots**, not literal output. Before emitting `globals.css`, resolve every `--color-*` token from the user-provided palette. The literal `oklch(...)` values shown here are illustrative only — emitting them verbatim when the user has supplied colors is a failure. If the user has NOT supplied a palette, run the Design Brief Script and ask; do not default to these placeholders.

**Token derivation order (no exceptions):**
1. **User-supplied tokens win.** Map every user color into a slot below by role. Match by intent (base = `surface`, text = `ink`, primary brand = `accent`, etc.), not by name.
2. **Theme polarity follows user direction.** If user picked light → `--color-surface` lightness ≥ `0.96`, `--color-ink` lightness `0.18`–`0.32`. If dark → invert. Never emit `--color-ink` with lightness `< 0.30` on a light page where user supplied a non-dark text color.
3. **Missing slots get derived from supplied ones.** `--color-surface-muted` = surface shifted by ±2% lightness. `--color-ink-muted` = ink lightness + 0.25, chroma halved. `--color-line` = surface shifted by 6–10% toward ink. Never invent unrelated hues.
4. **Skeleton placeholders below are emitted ONLY when user explicitly delegates ("알아서 해줘") AND project category map provides a default.** Otherwise replace each value with the user-derived counterpart.

```css
@import "tailwindcss";

@theme {
  --font-sans: var(--font-pretendard), "Geist", system-ui, sans-serif;
  --font-display: var(--font-display), "Geist", system-ui, sans-serif;

  /* ↓ PLACEHOLDERS — REPLACE FROM USER PALETTE BEFORE EMITTING ↓ */
  --color-surface: oklch(0.99 0.005 90);        /* user.base / page background */
  --color-surface-muted: oklch(0.96 0.005 90);  /* derived: surface ±2% L */
  --color-ink: oklch(0.22 0.02 270);            /* user.text / body — NEVER < 0.30 L on light themes */
  --color-ink-muted: oklch(0.47 0.015 270);     /* derived: ink + 0.25 L, chroma/2 */
  --color-line: oklch(0.90 0.004 90);           /* derived: borders/rings — neutral light */
  --color-accent: oklch(0.70 0.15 30);          /* user.accent */
  --color-accent-foreground: oklch(0.99 0.005 90); /* readable on accent */
  /* ↑ END PLACEHOLDERS ↑ */

  --ease-supanova: cubic-bezier(0.16, 1, 0.3, 1);
}

@layer base {
  html { scroll-behavior: smooth; }
  body { font-family: var(--font-sans); word-break: keep-all; }
}

@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(2rem); filter: blur(4px); }
  to   { opacity: 1; transform: translateY(0);    filter: blur(0); }
}

@keyframes float {
  0%, 100% { transform: translateY(0); }
  50%      { transform: translateY(-15px); }
}
```

### Required layout.tsx Skeleton
```tsx
import type { Metadata } from "next";
import localFont from "next/font/local";
import { Geist } from "next/font/google";
import "./globals.css";

const pretendard = localFont({
  src: "../public/fonts/PretendardVariable.woff2",
  variable: "--font-pretendard",
  display: "swap",
  weight: "45 920",
});

const geist = Geist({ subsets: ["latin"], variable: "--font-display" });

export const metadata: Metadata = {
  title: "페이지 제목",
  description: "페이지 설명",
};

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="ko" className={`${pretendard.variable} ${geist.variable}`}>
      <body className="bg-surface text-ink antialiased">{children}</body>
    </html>
  );
}
```

## 3. DESIGN ENGINEERING DIRECTIVES (Bias Correction)
LLMs have statistical biases toward specific UI cliches. These rules produce premium landing pages:

**Rule 1: Deterministic Typography**
* **Korean Headlines:** `text-4xl md:text-5xl lg:text-6xl tracking-tight leading-tight font-bold`. Pretendard renders Korean beautifully at these sizes.
  * **CRITICAL:** Korean text needs `leading-tight` to `leading-snug` (NOT `leading-none`). Korean glyphs need more vertical breathing room than Latin.
  * **Word Breaking:** Add `break-keep` to Korean text blocks to prevent mid-word breaks. Already set globally on `body` via `word-break: keep-all`, but reassert at the block level when needed.
* **English Display Text:** `tracking-tighter leading-none` for max impact with Latin fonts.
* **Body/Paragraphs:** `text-base md:text-lg text-ink-muted leading-relaxed max-w-[65ch]`. Never `text-gray-*` — use the derived ink-muted token so warm/cream palettes get warm-tinted body text, not cool slate.
* **ANTI-SLOP FONTS:** `Inter` BANNED. `Noto Sans KR` BANNED (use Pretendard — modern Korean standard). `Roboto`, `Arial`, `Open Sans` BANNED.

**Rule 2: Color Calibration**
* **Constraint:** Max 1 accent color per page. Saturation < 80%.
* **THE LILA BAN:** Purple/Blue "AI" gradients BANNED. No neon glows, no purple button effects.
* **USER THEME FIRST [CRITICAL]:** User's explicit theme and palette choices override defaults below as long as they don't violate accessibility or banned-pattern rules. If user provides colors, normalize into coherent base + one accent.
* **USER PALETTE = SOLE SOURCE OF TRUTH [HARD RULE]:** Once user supplies a palette, EVERY color emitted into `@theme`, JSX `className`, and inline style must derive from that palette via the token derivation order in the globals.css section. Banned: emitting `--color-ink: oklch(0.18 …)` when user gave a non-dark text color; using raw `bg-black/5`, `ring-black/5`, `bg-ink`, `text-ink` while the underlying `--color-ink` slot still holds the dark placeholder. Components reference semantic tokens (`bg-surface`, `text-ink`, `border-line`, `bg-accent`) and those tokens must already hold user-derived values before render.
* **NO HIDDEN DARK INJECTION [HARD RULE]:** `bg-black/*`, `ring-black/*`, `border-black/*`, `text-black`, `shadow-[*_rgba(0,0,0,*)]` are BANNED in source. They override the user palette by leaking pure black. Replace with `border-line`, `ring-line`, `bg-surface-muted`, or theme-tinted `shadow-[0_*_*_var(--color-ink)/0.06]`. Same for `bg-white/*` on dark themes — use `border-line` or surface tints.
* **ASK BEFORE DEFAULTING [CRITICAL]:** Do not choose palette unilaterally. Ask via Design Brief Script first for palette/accent. Theme (light/dark) defaults to LIGHT without asking unless user delegated full control.
* **DEFAULT IS LIGHT [HARD RULE]:** Base defaults to LIGHT. Dark is opt-in via explicit keyword ("dark", "다크", "어두운") OR category in {Gaming, Music, Cinema, Nightlife}. "Premium", "luxury", "cinematic", "minimal", "editorial" do NOT trigger dark. When the prompt is silent on theme, pick LIGHT — never charcoal/ink base.
* **CARD/SURFACE BRIGHTNESS [HARD RULE]:** On light theme, card backgrounds must be `#FFFFFF`, `#FAFAF7`, `#FBF9F4`, or other near-white from palette map. NEVER use `#0F0F12`, `#1A1A1A`, charcoal, or near-black card surfaces on a light page. Section dividers via subtle tone shifts only.
* **Token Wiring:** Encode the chosen palette as CSS variables inside `@theme` (e.g. `--color-surface`, `--color-surface-muted`, `--color-ink`, `--color-ink-muted`, `--color-accent`). Reference them as `bg-surface`, `text-ink`, `bg-accent`, etc. — never hardcode hex inside JSX.
* **PROJECT PALETTE MAP — fallback suggestions by project type:**
  * **Study / Education / Community / Productivity:** Warm cream `#FDFBF7` or off-white `#FAFAF7`. Accent: warm coral `#E8896B`, sage `#7A9E7E`, muted gold `#C9A961`.
  * **Finance / Fintech / B2B SaaS:** Cool off-white `#F7F8FA` or pale slate `#F1F3F5`. Accent: deep navy `#1E3A5F`, forest `#2D5F3F`, burnt orange `#D97757`.
  * **Health / Wellness / Medical:** Soft cream `#FBF9F4` or mint-tint `#F4F8F5`. Accent: sage `#7BA098`, terracotta `#C4806B`, sky `#7BAFD4`.
  * **Food / F&B / Lifestyle / Travel:** Warm beige `#F5EFE6` or paper `#FAF6EE`. Accent: espresso `#5C3D2E`, persimmon `#D4612F`, olive `#6B7A3E`.
  * **Dev Tools / AI / Tech SaaS:** Pure neutral `#FAFAFA` or warm white `#F9F8F6`. Accent: ink black `#1A1A1A`, electric blue `#3A6FF7`, chartreuse `#A8C66C`.
  * **Portfolio / Agency / Creative:** Bone `#F7F4EE` or pearl `#F8F8F6`. Accent: oxblood `#6E2C2C`, deep teal `#1F4E4A`, mustard `#C49A3A`.
  * **Beauty / Fashion / Luxury:** Champagne `#F5EDE0` or porcelain `#FCFAF6`. Accent: rose `#C18E8E`, espresso `#3D2E26`, gold `#B89968`.
  * **Gaming / Music / Cinema / Nightlife:** Dark base permitted — charcoal `#0F0F12` or deep ink `#0A0C14`. Accent: amber `#E8A547`, magenta `#D14B7F`, cyan `#5AC8D0`.
* **Palette Selection Process:** Collect or infer category, theme, base color, accent before writing code. If user supplied colors, use them after checking contrast and saturation. If not supplied, propose 2-3 options from map and ask. Only pick single fallback yourself when user explicitly delegates.
* **COLOR CONSISTENCY:** One palette for entire page. Never mix warm and cool grays.
* **Bright-Surface Discipline:** When base is light, body text uses `text-ink` (primary) and `text-ink-muted` (secondary) — never washed-out grays. Section dividers via subtle tone shifts (`bg-surface` → `bg-surface-muted`), never dark slabs. Both tokens already inherit the user's warm/cool palette character, so dividers stay tonally consistent.

**Rule 3: Landing Page Layout Diversification**
* **ANTI-CENTER BIAS:** When `DESIGN_VARIANCE > 4`, centered Hero sections BANNED. Use:
  * **Split Screen** (50/50 text + visual)
  * **Left-aligned content / Right-aligned asset**
  * **Asymmetric white-space** with dramatic negative space
  * **Full-bleed image with overlaid text**
* **Section Flow:** Landing page is NOT a stack of identical sections. Vary each section's layout dramatically:
  * Hero → Features (Bento Grid) → Social Proof (Testimonial Masonry) → CTA (Full-bleed)
  * Adjacent sections MUST use DIFFERENT layout patterns.
* **Component Boundaries:** Each section is its own file under `components/sections/`. Compose them inside `app/page.tsx`. This keeps reviewability high and lets the user swap sections.

**Rule 4: Materiality and Depth**
* Use cards (shadcn `<Card>` or custom Double-Bezel) ONLY when elevation communicates hierarchy. When shadows are needed, tint them to the background hue.
* **Glass Effects:** Go beyond `backdrop-blur`. Add `border border-line` and `shadow-[inset_0_1px_0_color-mix(in_oklab,var(--color-surface)_55%,white)]` for physical edge refraction. Never raw `border-white/10` or `rgba(255,255,255,*)` — those leak pure white that fights warm/cream surface tones.
* **Grain Texture:** Add subtle noise overlay via fixed `pointer-events-none` div rendered once in `layout.tsx`, not per-section.

**Rule 5: Conversion-Driven UI States**
* **CTA Buttons:** Built on shadcn `<Button>` with custom variant `supanova` defined in `components/ui/button.tsx`. Must have hover (`scale-[1.02]`), active (`scale-[0.98]`), focus-visible ring. Minimum `px-8 py-4 text-lg`.
* **Social Proof:** Numbers must feel organic (`47,200+` not `50,000+`). Real-sounding Korean names and companies.
* **Trust Signals:** Include at least one of: client logos, testimonial quotes, metrics bar, press mentions.
* **Urgency Elements (if conversion):** Subtle countdown, limited spots indicator, or "currently viewing" social proof — implemented as client component if dynamic.

**Rule 6: Korean Content Standards**
* **NO Translated Korean:** Native, natural Korean. "지금 시작하세요" not "시작을 하세요 지금".
* **Honorifics:** 합니다/하세요 form consistently. Never mix 반말 and 존댓말.
* **CTA Copy:** Direct, action-oriented: "무료로 시작하기", "3분만에 만들어보기", "지금 바로 체험하기".
* **Avoid Korean AI Cliches:** "혁신적인", "획기적인", "차세대" BANNED. Use concrete, specific language.

## 4. CREATIVE PROACTIVITY (Anti-Generic Implementation)
Systematically implement these high-end patterns as baseline:

* **"Liquid Glass" Refraction:** Beyond `backdrop-blur-xl`. Layer `border border-line`, `shadow-[inset_0_1px_0_color-mix(in_oklab,var(--color-surface)_50%,white)]`, subtle `bg-surface/60`. NEVER hardcode `rgba(255,255,255,*)` or `bg-white/*` — the highlight must mix from the user's surface token so warm/cream/dark palettes all render correctly.
* **Magnetic CTA Buttons:** Tailwind `transition-transform duration-500 ease-[cubic-bezier(0.16,1,0.3,1)]` with hover translate on the nested arrow icon. For real magnetic effect use `motion/react` `useMotionValue` + `useTransform` inside a client component.
* **Staggered Reveals:** Use `motion/react` `<motion.div initial={{opacity:0, y:32}} whileInView={{opacity:1, y:0}} viewport={{once:true}} transition={{delay: i*0.08, ease:[0.16,1,0.3,1]}}>` pattern. Wrap section as client component when needed.
* **Floating Elements:** `animate-[float_6s_ease-in-out_infinite]` referencing the `@keyframes float` declared in `globals.css`.
* **Gradient Mesh Backgrounds:** Multiple `radial-gradient` layers via Tailwind arbitrary values or a dedicated `<MeshBackground />` component.
* **Scroll-Triggered Animations (MOTION_INTENSITY > 6):** `useInView` from `motion/react`. NEVER `window.addEventListener('scroll')`.

## 5. PERFORMANCE GUARDRAILS
* **DOM Cost:** Grain/noise filter goes on ONE `fixed inset-0 pointer-events-none z-[60]` element in `layout.tsx`. Never on scrolling containers.
* **Hardware Acceleration:** Animate ONLY `transform` and `opacity`. Never `top`, `left`, `width`, `height`.
* **Image Optimization:** `next/image` handles `loading="lazy"` and `decoding="async"` automatically; always pass `width`, `height`, and `alt`. Mark only the hero image as `priority`.
* **Bundle Discipline:** Don't pull a full UI library. Only shadcn primitives you actually render. `@iconify/react` tree-shakes per icon import — fine.
* **Z-Index Restraint:** sticky nav (`z-40`), overlays (`z-50`), noise texture (`z-[60]`).
* **`"use client"` Discipline:** Don't mark the whole `page.tsx` as client. Keep server boundary high; push client wrappers down to leaf interactive components.

## 6. LANDING PAGE SECTION LIBRARY
Pull from this library of premium landing page patterns. Each becomes one file under `components/sections/`:

### Hero Sections
* **Split Hero:** 60/40 text-to-visual split. Text left, product screenshot or 3D render right. Background gradient bleed.
* **Full-Bleed Media Hero:** Full-screen `<Image fill />` or video with overlaid text. Gradient overlay (tone matched) for legibility. CTA floating at bottom-center.
* **Minimal Statement Hero:** Massive typography (`text-7xl+`) with extreme white-space. Single-line value prop. Floating CTA pill.
* **Interactive Hero:** Typewriter effect cycling use cases. "AI로 __ 만들기" with rotating words — client component using `motion/react` or simple `setInterval` in `useEffect`.

### Feature Sections
* **Bento Grid:** Asymmetric CSS Grid (`grid-cols-6` with mixed `col-span` and `row-span`).
* **Zig-Zag Alternating:** Image-left/text-right → text-left/image-right. Never 3-column equal cards.
* **Icon Strip:** Horizontal scrolling strip of feature icons with hover reveals.
* **Comparison Table:** "Before vs After" or "Us vs Them" with dramatic visual difference.

### Social Proof Sections
* **Logo Cloud:** Auto-scrolling marquee strip using CSS `@keyframes marquee` and `animation: marquee 30s linear infinite` on a duplicated row. Grayscale → color on hover.
* **Testimonial Masonry:** Staggered card heights. Real Korean names, real-feeling company names. `next/image` avatars.
* **Metrics Bar:** Large numbers with animated counting effect via `motion/react` `useInView` + `animate`.
* **Case Study Cards:** Before/after screenshots with overlay descriptions.

### CTA Sections
* **Full-Bleed CTA:** Solid palette-matched section, massive text, accent shadcn `<Button>`, floating trust badges below.
* **Sticky Bottom CTA:** `fixed bottom-0` bar that appears after scrolling past hero — client component using `useScroll`.
* **Inline CTA:** Embedded within content flow, styled differently from surrounding sections.

### Footer
* **Minimal Footer:** Logo, essential links, language selector, copyright. No 4-column link farms.
* **Rich Footer:** Brief company description, key nav links, social icons, newsletter signup.

## 7. AI TELLS (Forbidden Patterns)
To guarantee premium, non-generic output:

### Visual & CSS
* **NO Neon/Outer Glows.** Use inner borders or tinted shadows.
* **NO Pure Black (#000000) or Pure White (#ffffff) as raw surfaces.** Use slightly tinted neutrals from the chosen palette.
* **NO Oversaturated Accents.** Desaturate to blend with neutrals.
* **NO Excessive Gradient Text.** One gradient text element per page maximum.

### Typography
* **NO Inter, Noto Sans KR, Roboto, Arial.** Pretendard + premium English fonts only.
* **NO Oversized H1s without purpose.** Control hierarchy with weight and color, not just size.

### Layout
* **NO 3-Column Equal Card Rows.** Use Bento, zig-zag, or asymmetric layouts.
* **NO Identical Section Layouts.** Each section must have visually distinct structure.
* **NO Edge-to-Edge Content.** Always use `max-w-7xl mx-auto` container.

### Content
* **NO "John Doe" / "김철수".** Use creative realistic Korean names: "하윤서", "박도현", "이서진".
* **NO "Acme Corp" / "넥서스".** Invent premium Korean brand names: "스텔라랩스", "베리파이", "루미너스".
* **NO Round Numbers.** Use `47,200+` not `50,000+`. Use `4.87` not `5.0`.
* **NO AI Cliche Copy.** Ban: "혁신적인", "원활한", "차세대", "게임 체인저". Write specific, concrete copy.
* **NO Lorem Ipsum or English Placeholder.** All content in natural Korean.

### Code & Stack
* **NO `tailwind.config.js` with v3-style theme.** Use Tailwind v4 `@theme` in `globals.css`.
* **NO `<script src="https://cdn.tailwindcss.com">`.** That's the CDN runtime — wrong stack.
* **NO `<iconify-icon>` web component.** Use `@iconify/react` `<Icon />`.
* **NO `<img src="picsum.photos/...">`.** Use `next/image` with `remotePatterns` configured.
* **NO global `"use client"` at top of `page.tsx`.** Keep server boundary as high as possible.
* **NO Pages Router (`pages/index.tsx`) by default.** App Router only unless user explicitly asks otherwise.

### External Resources
* **NO Unsplash URLs.** Use `picsum.photos/seed/{name}/{w}/{h}` through `next/image`.

## 8. THE SUPANOVA LANDING PAGE FORMULA
When generating a complete landing page, follow this exact structure:

### A. Project Bootstrap (state assumed, do not re-output if scaffold exists)
```bash
npx create-next-app@latest supanova-landing --typescript --app --tailwind --eslint --src-dir=false --import-alias="@/*"
cd supanova-landing
npx shadcn@latest init -d
npx shadcn@latest add button card badge separator
npm i @iconify/react motion
```
`next.config.ts` must include:
```ts
import type { NextConfig } from "next";
const config: NextConfig = {
  images: {
    remotePatterns: [
      { protocol: "https", hostname: "picsum.photos" },
      { protocol: "https", hostname: "i.pravatar.cc" },
    ],
  },
};
export default config;
```

### B. Mandatory Section Order (Minimum)
1. **Navigation** — Floating glass pill OR minimal top bar (`components/sections/nav.tsx`)
2. **Hero** — Single most impactful section, above the fold
3. **Social Proof Strip** — Logo cloud or metrics bar
4. **Features** — 3-5 features in Bento or zig-zag
5. **Testimonials** — Real-feeling Korean testimonials with names and roles
6. **CTA** — Full-bleed conversion section with primary action
7. **Footer** — Minimal, clean, essential links only

### C. page.tsx Composition
```tsx
import { Nav } from "@/components/sections/nav";
import { Hero } from "@/components/sections/hero";
import { SocialProof } from "@/components/sections/social-proof";
import { Features } from "@/components/sections/features";
import { Testimonials } from "@/components/sections/testimonials";
import { CTA } from "@/components/sections/cta";
import { Footer } from "@/components/sections/footer";

export default function Page() {
  return (
    <main className="relative">
      <Nav />
      <Hero />
      <SocialProof />
      <Features />
      <Testimonials />
      <CTA />
      <Footer />
    </main>
  );
}
```

### D. Design Philosophy
* **Premium by Default:** Every pixel intentional. Looks like a template → fails.
* **Korean-Native:** Page must feel designed BY Koreans FOR Koreans. Not translation.
* **Conversion-Focused:** Every section guides eye toward CTA. Visual hierarchy = conversion funnel.
* **Mobile-First:** 70%+ Korean web traffic is mobile. Design mobile-first, enhance for desktop.

## 9. FINAL PRE-FLIGHT CHECK
Evaluate before output:
- [ ] Next.js 15 App Router project with `app/layout.tsx`, `app/page.tsx`, `app/globals.css`?
- [ ] Tailwind v4 via `@import "tailwindcss"` and `@theme` block — no CDN script, no v3 config file?
- [ ] Pretendard loaded via `next/font/local`, set as primary font through `--font-pretendard` and `@theme`?
- [ ] shadcn `<Button>`, `<Card>` etc. used where appropriate, installed in `components/ui/`?
- [ ] All icons via `@iconify/react` Solar set — no `<iconify-icon>` web component?
- [ ] All images via `next/image` with `remotePatterns` configured for picsum/pravatar?
- [ ] All visible text written in natural Korean?
- [ ] `break-keep-all` (via global `word-break: keep-all`) applied to Korean blocks?
- [ ] Full-height sections use `min-h-[100dvh]`, not `h-screen`?
- [ ] Mobile layout (`w-full`, `px-4`) guaranteed for all sections?
- [ ] CTA buttons meet 48px+ mobile tap target?
- [ ] Each section uses DIFFERENT layout pattern from neighbors?
- [ ] Zero banned fonts, zero emoji, zero Unsplash links?
- [ ] `"use client"` confined to interactive leaf components?
- [ ] Page feels premium, not template-like?
