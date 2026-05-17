---
name: supanova-full-output
description: Overrides default LLM truncation behavior. Enforces complete Next.js project generation with zero placeholder patterns. Every landing page must be delivered as a complete, production-ready Next.js App Router project — every required file, every section component, every responsive breakpoint. No shortcuts, no skeletons, no "add more as needed" patterns.
---

# Supanova Full-Output Enforcement

## Baseline

Treat every landing page generation as production-critical. A partial output is a broken output. If user asks for a landing page, deliver the COMPLETE Next.js project — every required file (`app/layout.tsx`, `app/page.tsx`, `app/globals.css`, every section component, `next.config.ts`, `tsconfig.json` when missing, `package.json` deltas), every section, every animation, every responsive breakpoint. No exceptions.

## Banned Output Patterns

The following patterns are hard failures. Never produce them:

**In code blocks:**
- `{/* ... */}`
- `{/* rest of sections */}`
- `{/* similar to above */}`
- `{/* add more sections as needed */}`
- `// TODO`
- `// ...`
- Bare `...` standing in for omitted JSX or TS
- `// implementation here`
- Empty function bodies returning `null` as a stand-in for a real section
- Importing a component (`import { Features } from "..."`) without ever writing the component file

**In prose:**
- "Let me know if you want me to continue"
- "I can add more sections if needed"
- "For brevity, I'll show just the hero section"
- "The rest follows the same pattern"
- "Similarly for the remaining sections"
- "I'll leave that for you to customize"
- "You can run `npx shadcn add ...` to get the rest" — install commands are fine, but section JSX must still be fully written

**Structural shortcuts:**
- Outputting only the Hero when a full page was requested
- Showing the first and last section, skipping the middle
- Writing one section component fully and stubbing the others
- Describing what JSX should contain instead of writing it
- Generating a wireframe/skeleton when a complete page was requested
- Skipping `app/globals.css` token wiring because "Tailwind defaults are enough"
- Skipping `next.config.ts` `remotePatterns` after using `next/image` with `picsum.photos`

## Execution Process

1. **Scope** — Read full request. Count expected sections/components. "Landing page" means at minimum: nav + hero + social proof + features + testimonials + CTA + footer (7 section components) PLUS `app/layout.tsx`, `app/page.tsx`, `app/globals.css`, `next.config.ts`, and any shadcn primitives referenced (`components/ui/button.tsx` etc.). Lock the file list.
2. **Build** — Generate every file completely. Every section component is a real, exported, typed React component with full responsive classes, motion, real Korean content, and proper Iconify icons.
3. **Cross-check** — Before output, verify:
   - Every file declared in the file list above exists in the response.
   - `app/page.tsx` imports only components that have actual JSX definitions in the response.
   - `app/layout.tsx` has `<html lang="ko">`, font wiring, and `import "./globals.css"`.
   - `app/globals.css` has `@import "tailwindcss"` and `@theme` block.
   - Every section component has 7+ sections fully populated when composed into the page.

## Handling Long Outputs

When generation approaches the token limit:

- Do NOT compress remaining files to fit.
- Do NOT skip to the footer or to `package.json`.
- Write at full quality up to a clean breakpoint (end of a complete file or a complete `</section>` JSX subtree).
- End with:

```
[PAUSED — X of Y files complete. Send "continue" to resume from: components/sections/<next>.tsx]
```

On "continue", pick up at the next file exactly where you stopped. No recap, no re-outputting `layout.tsx`, no repetition.

## Project Completeness Standards

A complete Supanova Next.js landing page MUST include:

### Required Files
- `app/layout.tsx` — `<html lang="ko">`, `next/font` for Pretendard + display font, metadata, `import "./globals.css"`, optional global noise overlay
- `app/page.tsx` — server component composing all sections
- `app/globals.css` — `@import "tailwindcss"`, `@theme` palette + font tokens, `@keyframes` for fadeInUp/float/marquee, base layer with `word-break: keep-all`
- `next.config.ts` — `images.remotePatterns` for `picsum.photos` and `i.pravatar.cc` if used
- `components/sections/nav.tsx`
- `components/sections/hero.tsx`
- `components/sections/social-proof.tsx`
- `components/sections/features.tsx`
- `components/sections/testimonials.tsx`
- `components/sections/cta.tsx`
- `components/sections/footer.tsx`
- `components/ui/<name>.tsx` for every shadcn primitive referenced in JSX (don't `import { Button } from "@/components/ui/button"` without providing the file when the user lacks a scaffold)
- `lib/utils.ts` — shadcn `cn()` helper if `cn(...)` is called

### Required Quality
- Every section component has real Korean content (no placeholder text, no `lorem`, no English filler)
- Every section has full responsive classes (`sm:`, `md:`, `lg:`)
- Every interactive element has hover/active/focus-visible states
- Every `next/image` has explicit `width`, `height` (or `fill` with a sized parent), and Korean `alt`
- Every icon uses `<Icon icon="solar:..." />` from `@iconify/react`
- Every client-only section is marked with `"use client"` at the top of that file — and only that file
- TypeScript: every component exports a typed function, no `any`, props typed as `interface` or inline `type`

## Quick Check

Before finalizing any response, verify:
- No banned patterns from the list above appear anywhere
- The Next.js project compiles in principle: every imported identifier has a definition somewhere in the response
- `app/layout.tsx` → `app/page.tsx` → all 7 section components are present and fully populated
- Every code block contains actual runnable TS/TSX/CSS, not descriptions
- Nothing was shortened, summarized, or omitted to save space
- All visible text content is in natural Korean
- No CDN script tags for Tailwind, no `<iconify-icon>` web component, no raw `<img>` tag pointing at picsum
