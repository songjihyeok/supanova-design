# Supanova Design Skill

AI가 생성하는 Next.js 랜딩페이지의 디자인 퀄리티를 극적으로 향상시키는 스킬 모음입니다. 제네릭한 AI 템플릿 대신 $150k 에이전시 수준의 프리미엄 랜딩페이지를 Next.js App Router + Tailwind CSS v4 + shadcn/ui 스택으로 생성합니다.

> **Powered by [supanova.dev](https://supanova.dev)** — AI 랜딩페이지 빌더
>
> Based on [taste-skill](https://github.com/Leonxlnx/taste-skill) by [@lexnlin](https://x.com/lexnlin)

## Skills

4개의 스킬이 각각의 폴더에 `SKILL.md` 파일로 존재합니다.

### 1. taste-skill (Supanova Design Engine)
메인 디자인 스킬. AI가 처음부터 프리미엄 Next.js 랜딩페이지를 생성하도록 가르칩니다. 레이아웃, 타이포그래피, 컬러, 모션, 한국어 콘텐츠 품질, 그리고 Next.js 파일 구조까지 포괄합니다.

### 2. redesign-skill (Supanova Redesign Engine)
기존 Next.js 랜딩페이지를 업그레이드합니다. 처음부터 다시 만드는 대신, 현재 디자인을 진단하고 가장 임팩트 있는 개선을 우선 적용합니다. Tailwind CDN/v3 → v4 마이그레이션, `<iconify-icon>` → `@iconify/react` 같은 스택 정리도 포함합니다.

### 3. soft-skill (Supanova Premium Aesthetic)
$150k 에이전시 퀄리티에 집중합니다. Double-Bezel 카드 아키텍처, motion/react 기반 스프링 애니메이션, 플로팅 글래스 네비게이션, 한국어 타이포그래피 표준, shadcn `<Button>` Supanova 변형을 정의합니다.

### 4. output-skill (Supanova Full-Output)
AI의 출력 생략을 방지합니다. 플레이스홀더, 스켈레톤, 미완성 출력을 차단하고, Next.js 프로젝트의 모든 필수 파일(`app/layout.tsx`, `app/page.tsx`, 모든 섹션 컴포넌트, `next.config.ts`, `globals.css`)을 완전한 코드로 출력하도록 강제합니다.

## Supanova 특화 포인트

원본 taste-skill과의 주요 차이점:

- **Next.js App Router 출력** — 단일 HTML이 아닌 production-grade Next.js 프로젝트 (App Router + RSC)
- **Tailwind CSS v4** — `@import "tailwindcss"` + `@theme` 토큰. v3 config 파일 사용 안 함
- **shadcn/ui 프리미티브** — `<Button>`, `<Card>`, `<Badge>` 등을 Supanova 미감으로 확장
- **한국어 퍼스트** — Pretendard via `next/font/local`, `word-break: keep-all`, 자연스러운 한국어 카피
- **Iconify Solar via `@iconify/react`** — React 친화적 아이콘 시스템 (웹 컴포넌트 아님)
- **motion/react** — 스프링 기반 모션, `useInView` 스크롤 트리거
- **랜딩페이지 특화** — 일반 웹앱이 아닌 전환율 중심 랜딩페이지 패턴
- **한국 시장** — 한국 사용자 이름, 한국 기업명, 한국어 CTA 패턴

## 사용법

1. 필요한 스킬의 `SKILL.md` 파일을 프로젝트에 복사합니다.
2. AI 에디터에서 해당 파일을 참조하세요. (예: Cursor에서 `@SKILL.md`, Claude Code에서 스킬 등록)

끝입니다. AI가 파일을 읽고 규칙을 따릅니다.

### 추천 조합

| 상황 | 추천 스킬 |
|------|-----------|
| 새 Next.js 랜딩페이지 생성 | `taste-skill` + `output-skill` |
| 기존 Next.js 페이지 업그레이드 | `redesign-skill` |
| 최고 퀄리티가 필요할 때 | `taste-skill` + `soft-skill` + `output-skill` |

## 설정 (taste-skill)

taste-skill은 페이지를 만들기 전에 목적, 카테고리, 테마, 컬러, 모션, 밀도, 참고 스타일을 사용자에게 먼저 묻도록 설정되어 있습니다. 사용자가 "알아서", "추천해줘", "자동으로 만들어줘"처럼 위임하면 그때만 카테고리 기반 팔레트 맵을 fallback으로 사용합니다.

taste-skill 상단의 설정값을 조정할 수 있습니다:

**DESIGN_VARIANCE** — 레이아웃의 실험성
- 1-3: 깔끔하고 정돈된 대칭 그리드
- 4-7: 오버랩, 다양한 사이즈
- 8-10: 비대칭, 넉넉한 여백, 모던

**MOTION_INTENSITY** — 애니메이션 수준
- 1-3: 거의 없음. 호버 효과 정도
- 4-7: 페이드인, 스무스 스크롤
- 8-10: 마그네틱 효과, 스프링 물리, 스크롤 트리거

**VISUAL_DENSITY** — 화면당 콘텐츠 밀도
- 1-3: 넓고 럭셔리. 한 번에 하나의 요소
- 4-7: 일반적인 앱/웹사이트 간격
- 8-10: 촘촘하고 데이터 중심

**LANDING_PURPOSE** — 페이지 목적
- conversion: 전환율 중심 (기본값)
- brand: 브랜드 이미지 중심
- portfolio: 포트폴리오/쇼케이스
- saas: SaaS 제품 소개
- ecommerce: 이커머스/제품 판매

**SURFACE_MODE / PROJECT_CATEGORY**
- 기본값은 `ask-first`입니다.
- 사용자가 밝은/어두운 테마나 카테고리를 명시하지 않으면 스킬이 먼저 질문합니다.
- 사용자가 결정을 위임한 경우에만 카테고리별 추천 팔레트를 선택합니다.

## 기술 스택

이 스킬로 생성되는 프로젝트의 기본 스택:

- **Framework:** Next.js 15 (App Router) + React 19 + TypeScript
- **Styling:** Tailwind CSS v4 (`@tailwindcss/postcss`) + `@theme` 토큰
- **Components:** shadcn/ui (필요한 프리미티브만 설치)
- **Korean Font:** Pretendard via `next/font/local`
- **Display Font:** Geist via `next/font/google` (또는 Outfit / Cabinet Grotesk / Satoshi)
- **Icons:** `@iconify/react` Solar 세트
- **Motion:** `motion/react` (formerly Framer Motion)
- **Images:** `next/image` (picsum.photos / i.pravatar.cc는 `next.config.ts` `remotePatterns`에 추가)

### 부트스트랩

```bash
npx create-next-app@latest supanova-landing --typescript --app --tailwind --eslint --import-alias="@/*"
cd supanova-landing
npx shadcn@latest init -d
npx shadcn@latest add button card badge separator
npm i @iconify/react motion
```

`next.config.ts`:
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

`app/globals.css` 스켈레톤:
```css
@import "tailwindcss";

@theme {
  --font-sans: var(--font-pretendard), "Geist", system-ui, sans-serif;
  --color-surface: oklch(0.99 0.005 90);
  --color-ink: oklch(0.18 0.02 270);
  --color-accent: oklch(0.7 0.15 30);
  --ease-supanova: cubic-bezier(0.16, 1, 0.3, 1);
}

@layer base {
  html { scroll-behavior: smooth; }
  body { font-family: var(--font-sans); word-break: keep-all; }
}
```

## 기여 & 피드백

- GitHub Issue 또는 Pull Request
- [supanova.dev](https://supanova.dev)

## 라이선스

원본 [taste-skill](https://github.com/Leonxlnx/taste-skill)의 라이선스를 따릅니다.
