# performative-ui 전수조사 및 활용 전략 (한국어 정리)

> 이 문서는 performative-ui 레포지토리를 전수조사한 결과와, 설치/사용법,
> 분류(플러그인 vs 스킬 vs MCP), API 토큰 필요 여부, AI 에이전트 구축
> 활용성, 수익화 전략, 프레임워크 이식 가능성, 콘텐츠 제작 가능성을
> 한국어로 정리한 기록입니다.
>
> 작성일: 2026-10-01

## 목차

1. [레포지토리 기본 정보](#1-레포지토리-기본-정보)
2. [이 프로젝트의 정체](#2-이-프로젝트의-정체)
3. [폴더 구조 상세](#3-폴더-구조-상세)
4. [컴포넌트 33개 전체 목록](#4-컴포넌트-33개-전체-목록)
5. [코드 품질 평가](#5-코드-품질-평가)
6. [설치 및 사용법](#6-설치-및-사용법)
7. [분류: 플러그인인가 스킬인가 MCP인가](#7-분류-플러그인인가-스킬인가-mcp인가)
8. [API 토큰 필요 여부](#8-api-토큰-필요-여부)
9. [AI 에이전트 구축 활용성](#9-ai-에이전트-구축-활용성)
10. [React / PHP 구현 가능성](#10-react--php-구현-가능성)
11. [유튜브 콘텐츠 제작 가능성](#11-유튜브-콘텐츠-제작-가능성)
12. [수익화 아이디어 7종](#12-수익화-아이디어-7종)
13. [추천 로드맵](#13-추천-로드맵)
14. [참고 링크](#14-참고-링크)

---

## 1. 레포지토리 기본 정보

| 항목 | 내용 |
| --- | --- |
| 작업 레포 | https://github.com/bmshin94/performativeUI |
| 원본 레포 | https://github.com/vorpus/performativeUI |
| npm 패키지 | `performative-ui` v0.7.0 (public) |
| 공식 문서 | https://performative-ui.cncl.co/ |
| npm 페이지 | https://www.npmjs.com/package/performative-ui |
| 라이선스 | MIT (상업적 이용 가능) |
| 기술 스택 | React 19, TypeScript strict, Vite 6 |
| 런타임 의존성 | 없음 (peerDependencies로 react, react-dom만) |
| 규모 | 컴포넌트 33개, src 약 6,450줄, styles.css 2,045줄, research 2,876줄 |
| 번들 크기 | 약 30KB |
| 작업 브랜치 | `claude/funny-mayer-lh4wcn` |

한 줄 소개: "AI-native React components that signal how oversubscribed your
funding round is." (우리 투자 라운드가 얼마나 초과청약됐는지 과시해주는
AI-네이티브 React 컴포넌트)

## 2. 이 프로젝트의 정체

AI 스타트업 랜딩페이지에서 반복적으로 등장하는 시각적 클리셰를 수집해
실제로 사용 가능한 React 컴포넌트로 구현한 **패러디 디자인 시스템**입니다.

풍자이지만 제품 품질은 진지합니다. 타입 안정성, 접근성 속성, 테마 토큰,
메모리 누수 방지 처리가 모두 들어 있습니다.

### 패턴 등록 조건

`research/05_aiified_ui_elements.md`에 명시된 수록 기준이 이 프로젝트의
정체성을 가장 잘 설명합니다.

1. 시각 또는 문구 레벨만 바꾸는 것이어야 한다 (새 기능 추가 없음).
2. 제거해도 제품 동작은 변하지 않아야 한다.
3. 그런데 제거하면 사용자가 "이게 AI 제품인가" 확신하지 못하게 된다.

즉 "기능은 늘지 않지만, 없으면 AI처럼 보이지 않는 요소들"의 도감입니다.

### research 폴더: 숨은 핵심 자산

실제 AI 스타트업 랜딩페이지를 전수조사한 필드 리포트 7개 문서,
총 2,876줄입니다.

| 문서 | 내용 |
| --- | --- |
| `00_source_companies.md` | YC W24~W26 + 비YC 명예의 전당 AI 회사 랜딩페이지 마스터 카탈로그 |
| `01_typewriter_hero.md` | 타이프라이터 헤드라인 분류학. 사용 라이브러리까지 추적 |
| `02_logo_walls.md` | "Trusted by" 로고월 필드노트. 실제 로고 리스트 포함 |
| `03_node_graph_backgrounds.md` | 노드그래프 배경 25개+ 실측. 소스 스니핑 결과 포함 |
| `04_ascii_hero_art.md` | ASCII 히어로 아트 정경(canon) |
| `05_aiified_ui_elements.md` | AI화된 UI 베스티어리. 수록 기준 명시 |
| `06_cursor_effects.md` | 커서 이펙트 분류 (presence / replace / spotlight / trail) |

디자인 트렌드 리서치 자료로서 그 자체로 높은 가치가 있습니다.

## 3. 폴더 구조 상세

```
src/                     npm으로 배포되는 라이브러리 본체
  components/ (33개)     컴포넌트 1개당 파일 1개
  hooks/ (4개)           useTypewriter, useCounter, useTokenStream, useAsciiField
  utils/cn.ts            className 합치는 유틸 (25줄, clsx 대체)
  index.ts               public export 배럴 (컴포넌트 + 타입)
  styles.css             전체 스타일. 전부 .pui-* 접두사 (2,045줄)
docs/                    문서 사이트 SPA (React Router v7)
  lib/meta.tsx           컴포넌트 카탈로그 (snark, 예제, props표, 실제 사례 링크)
  lib/ComponentPage.tsx  컴포넌트별 문서 페이지 템플릿
  lib/PropsTable.tsx     props 표 렌더러
  lib/CodeBlock.tsx      코드 블록
  lib/Attribution.tsx    출처 표기
  lib/meta.tsx           CATEGORY_ORDER, CATEGORIES, ORDERED_COMPONENTS 정의
  pages/Home.tsx         랜딩페이지 (자기 자신의 컴포넌트로 제작됨)
  App.tsx                사이드바, 라우팅, 테마 토글, [ ] 스킴 핫키
demo/                    최초 프로토타입 (바닐라 HTML/JS). 보존용, npm 미포함
  js/typewriter.js       Rotator 로직 순수 JS 버전
  js/streams.js          TokenStream 로직
  js/ascii-hero.js       AsciiHero Canvas
  js/node-graph.js       NodeGraph Canvas
research/                AI 랜딩페이지 조사 문서 7개
scripts/gen-og.mjs       OG 공유 이미지 생성 (resvg)
.github/workflows/pages.yml   main 푸시 시 GitHub Pages 자동 배포
AGENTS.md                기여 가이드 (사람 + AI 에이전트용)
CLAUDE.md                AGENTS.md 참조 + 페르소나 설정
vercel.json              Vercel 배포 설정
vite.lib.config.ts       라이브러리 빌드 설정
vite.docs.config.ts      문서 사이트 빌드 설정
```

문서 사이트가 자기 자신의 컴포넌트로 만들어진 점이 특징입니다.
`docs/pages/Home.tsx`가 `AsciiHero`, `Aurora`, `Rotator`, `GradientText`,
`StickyBanner`, `EyebrowPill`을 실제로 import해서 사용합니다.

## 4. 컴포넌트 33개 전체 목록

사이드바 순서는 `docs/lib/meta.tsx`의 `CATEGORY_ORDER`로 명시 제어됩니다.

### Heroes (6개)

| 컴포넌트 | 설명 | snark |
| --- | --- | --- |
| `AsciiHero` | 마우스 반응형 ASCII 아트 히어로 | For hackers, by people who follow the right newsletters. |
| `Goldeneye` | 호버 시 드러나는 히어로 | Make them earn it. |
| `Rotator` | 단어가 타이핑되며 순환 | Because saying "everything" wasn't ambitious enough. |
| `WordRoll` | 단어가 굴러가며 교체 | All the breadth-flexing of a Rotator, without making the visitor wait for it to type. |
| `PromptHero` | 가치 제안 대신 입력창 | We replaced the value prop with a text input. |
| `Prompt` | 그 입력창 자체 (Primitives 분류) | The textarea every AI builder ships instead of explaining what their product does. |

### Menus (1개)

| 컴포넌트 | 설명 | snark |
| --- | --- | --- |
| `Temperature` | 모델 창의성 슬라이더 | These go to eleven. |

### Social Proof (5개)

| 컴포넌트 | 설명 | snark |
| --- | --- | --- |
| `LogoMarquee` | 무한 스크롤 로고월 | (긴 문장) |
| `LogoRow` | 정적 로고 줄 | Static logos are for when you only have six. |
| `SlippyWords` | 스크롤 시 움직이는 버즈워드 | Buzzwords that physically move when you scroll. Motion design, allegedly. |
| `StatCounter` | 숫자 증가 카운터 | Numbers that go up are better than numbers that don't. |
| `CommunityBadge` | GitHub 스타 배지 | Stars are the new MAU. |

### Atoms (4개)

| 컴포넌트 | 설명 | snark |
| --- | --- | --- |
| `Sparkle` | 반짝이 글리프 | Add a sparkle to any noun to ship it twice as fast. |
| `GradientText` | 그라데이션 텍스트 | When italic isn't billion-dollar enough. |
| `StatusDot` | 상태 표시 점 | Always green, even when it's not. |
| `QuestText` | 레트로 픽셀 채팅 텍스트 | you never quit, you just took a break |

### Primitives (3개)

| 컴포넌트 | 설명 | snark |
| --- | --- | --- |
| `Button` | 폴리모픽 버튼. glow / shimmer / ghost / solid / wave 5변형 | We made the button move so you'd click the button. |
| `EyebrowPill` | 작은 라벨 필 | Where the model name goes when there's nothing else to say. |
| `Prompt` | 프롬프트 텍스트에어리어 | (Heroes 표 참조) |

### Banners (1개)

| 컴포넌트 | 설명 | snark |
| --- | --- | --- |
| `StickyBanner` | 상단 고정 배너 | Funding news disguised as utility. |

### Backgrounds (3개)

| 컴포넌트 | 설명 | snark |
| --- | --- | --- |
| `Aurora` | 블러 블롭 배경 | Three blobs and a generation defined. |
| `NodeGraphBackground` | 점과 선의 노드그래프 Canvas | A neural network, conceptually. |
| `FloatingSparkles` | 떠다니는 반짝이 | Magic doesn't ship itself. |

### Surfaces (2개)

| 컴포넌트 | 설명 | snark |
| --- | --- | --- |
| `GlassCard` | 글래스모피즘 카드 | Backdrop-filter: ambition. |
| `MockIDE` | 가짜 IDE 화면 (토큰 하이라이팅) | Real code is coming. This is the trailer. |

### Conversation (4개)

| 컴포넌트 | 설명 | snark |
| --- | --- | --- |
| `ChatBubble` | 대화 말풍선 (user / assistant 역할) | If it's in a bubble, it must be true. |
| `TokenStream` | 토큰 단위 스트리밍 표시 | Server-sent events (SSE) were added to the HTML5 spec in 2008 but never used until 2025. |
| `WibblingSpinner` | Claude Code 스피너 재현. 동사 186개 + 글리프 6개 내장 | If you haven't replaced yours with a banner ad yet. |
| `ChatFAB` | 우하단 챗봇 플로팅 버튼 | There's no escape now. |

### Pricing & Conversion (4개)

| 컴포넌트 | 설명 | snark |
| --- | --- | --- |
| `PricingCard` | 가격 카드 | The middle one is glowing. Choose accordingly. |
| `BeforeAfter` | 전후 비교 | On the left: chaos. On the right: us. |
| `WaitlistForm` | 대기명단 폼 | Demand we manufactured ourselves. |
| `Popover` | 팝오버 | Built for conversion, not consent. |

### Footers (1개)

| 컴포넌트 | 설명 |
| --- | --- |
| `BigBack` | 거대한 멀티컬럼 푸터 |

### Hooks (4개)

| 훅 | 역할 |
| --- | --- |
| `useTypewriter` | 타이핑 및 삭제 애니메이션 |
| `useCounter` | 숫자 증가 애니메이션 |
| `useTokenStream` | 토큰 단위 텍스트 공개 |
| `useAsciiField` | ASCII 문자 필드 계산 |

컴포넌트는 대부분 이 훅들의 얇은 래퍼입니다. 마크업을 직접 제어하고 싶으면
훅만 가져다 쓰면 됩니다.

## 5. 코드 품질 평가

### 좋은 점

- **헤드리스 훅 분리**: `TokenStream`은 `useTokenStream()`의 래퍼.
  로직과 표현이 깔끔히 분리되어 있어 실제 API 연동 시 재활용하기 쉽습니다.
- **폴리모픽 컴포넌트**: `<Button as="a" href="/x">` 가능.
  Radix UI, Chakra UI가 쓰는 제네릭 forwardRef 패턴을 정석대로 구현.
- **CSS 토큰 테마**: 모든 색이 `var(--pui-bg)` 형태의 커스텀 프로퍼티.
  `data-theme="light"` 한 줄로 전체 테마 전환.
- **접근성**: `aria-busy`, `aria-hidden`, `disabled` 처리가 들어 있음.
- **의존성 0개**: three.js, GSAP, Framer Motion 없이 순수 CSS + Canvas +
  setTimeout으로 구현. 전체 약 30KB.
- **cleanup 철저**: 모든 훅이 `cancelled` 플래그 + `clearTimeout`으로
  언마운트 시 타이머를 정리. 메모리 누수 없음.

### 주의할 점

- `useTokenStream`의 `useEffect` 의존성 배열이 `[]`입니다
  (`eslint-disable-next-line react-hooks/exhaustive-deps`로 억제).
  `text` prop이 변경되어도 재실행되지 않습니다.
  동적 텍스트를 쓰려면 `key` prop으로 강제 리마운트하거나,
  state를 직접 관리해야 합니다.
- `docs/pages/Home.tsx`가 매 마운트마다 `buttons.github.io/buttons.js`
  script 태그를 새로 append합니다. 해당 스크립트가 공개 API 없는 IIFE라서
  재스캔을 유도하기 위한 의도적 처리이며, 주석에 이유가 적혀 있습니다.

## 6. 설치 및 사용법

### 방법 A: npm 패키지로 사용

```bash
npm create vite@latest my-landing -- --template react-ts
cd my-landing
npm install
npm install performative-ui
```

React 18 또는 19가 필요합니다
(`peerDependencies: "react": "^18.0.0 || ^19.0.0"`).

CSS를 명시적으로 불러옵니다. `src/index.ts`가 `./styles.css`를 자동
import하지만, 번들러 설정에 따라 누락될 수 있으므로 직접 쓰는 편이
안전합니다. `package.json`의 `exports`에 `"./styles.css"` 경로가 별도로
열려 있는 이유입니다.

```tsx
import "performative-ui/styles.css";
```

사용 예시:

```tsx
import {
  Aurora, Rotator, GradientText, Button,
  LogoMarquee, PricingCard, WaitlistForm,
} from "performative-ui";
import "performative-ui/styles.css";

export default function App() {
  return (
    <main style={{ position: "relative", minHeight: "100vh" }}>
      <Aurora />
      <h1>
        세상에서 가장 야심찬{" "}
        <GradientText>
          <Rotator words={["에이전트", "코파일럿", "워크플로우", "뭐든지"]} />
        </GradientText>
      </h1>
      <Button variant="glow" sparkle>지금 시작하기</Button>
      <WaitlistForm />
    </main>
  );
}
```

테마 전환은 조상 요소에 `data-theme`를 지정합니다.

```html
<html data-theme="light"><!-- 생략하면 기본 다크 --></html>
```

### 방법 B: 레포 직접 실행

```bash
npm install          # node_modules가 없으므로 먼저 실행
npm run dev          # http://localhost:5174, HMR 적용
```

| 명령어 | 역할 |
| --- | --- |
| `npm run dev` | 문서 사이트 개발 서버 (5174 포트) |
| `npm run typecheck` | `tsc --noEmit` 타입 검사 |
| `npm run build:lib` | npm 배포용 번들 생성 (`dist/`) |
| `npm run build:docs` | 문서 사이트 빌드 (`dist-docs/`) |
| `npm run build` | 위 둘 다 |
| `npm run build:og` | OG 공유 이미지 생성 |
| `npm run preview` | 빌드 결과 미리보기 |

### 새 컴포넌트 추가 절차 (AGENTS.md 기준)

1. `src/components/MyComponent.tsx` 생성.
   `src/styles.css`에 `.pui-mycomponent` 접두사로 스타일 작성.
   색상은 반드시 기존 CSS 커스텀 프로퍼티 사용.
2. `src/index.ts`에서 re-export. props 타입도 함께 export.
3. `docs/lib/meta.tsx`에 문서 항목 추가.
   **같은 카테고리의 다른 항목들 옆자리에 삽입**해야 합니다.
   위치가 틀리면 `[` `]` 스킴 핫키와 하단 "Next" 카드의 순서가 깨집니다.
4. 랜딩페이지의 컴포넌트 개수(`{COMPONENTS.length} components`)는
   자동 갱신됩니다. 숫자를 수동으로 바꾸지 않습니다.

### 레포 고유 컨벤션

- em dash 사용 금지 (문서, 주석, README, 컴포넌트 설명 전부).
  쉼표나 마침표로 대체하거나 문장을 재구성합니다.
  정기적으로 sed 스타일 스윕을 돌린다고 명시되어 있습니다.
- 코드와 마크다운에 이모지 금지. 단 Sparkle처럼 이모지 글리프 자체가
  기능인 경우는 예외입니다.
- 톤: 래퍼 문구는 진지한 AI 스타트업 언어로 작성하고, 농담은 컴포넌트
  `snark` 한 줄에서만 터뜨립니다. 제목이나 리드에서 미리 알려주지 않습니다.
- 하드코딩 색상 금지. 새 토큰이 필요하면 다크 블록과
  `[data-theme="light"]` 블록 양쪽에 모두 추가합니다.

### 배포

| 대상 | 방법 |
| --- | --- |
| GitHub Pages | `main` 푸시 시 `.github/workflows/pages.yml`이 자동 배포 (약 30초) |
| Vercel | `vercel.json` 기준. 빌드 `npm run build:docs`, 출력 `dist-docs` |
| npm | `npm version minor` → `npm run build:lib` → `npm publish` → `git push origin main --follow-tags` |

`npm publish`는 `dist/`만 올립니다. `build:lib` 없이 publish하면
오래된 코드가 배포되므로 순서를 지켜야 합니다.

## 7. 분류: 플러그인인가 스킬인가 MCP인가

결론은 세 가지 모두 아니며, **일반 npm React 컴포넌트 라이브러리**입니다.

| 분류 | 정체 | 실행 위치 | performative-ui |
| --- | --- | --- | --- |
| npm 라이브러리 | 코드 패키지, import해서 사용 | 브라우저 | 해당 |
| Claude Code 플러그인 | 스킬, 커맨드, 훅, MCP 서버 번들 | Claude Code 내부 | 해당 없음 |
| 스킬 (Skill) | SKILL.md + 스크립트. AI에게 작업 방법 제공 | Claude / Claude Code 내부 | 해당 없음 |
| MCP 서버 | Model Context Protocol 서버. AI에 도구 제공 | 별도 프로세스 | 해당 없음 |

### 근거 (파일 확인 결과)

없는 것:

- `.claude/skills/` 디렉터리 없음, `SKILL.md` 없음
- `.claude-plugin/plugin.json` 없음
- `mcp.json` 없음, `@modelcontextprotocol/sdk` 의존성 없음
- `package.json`에 `bin` 필드 없음 (CLI 도구 아님)

있는 것:

- `main`, `module`, `types`, `exports` 필드 (전형적인 라이브러리 패키지)
- `peerDependencies`에 react, react-dom (React 컴포넌트 라이브러리)

`CLAUDE.md`와 `AGENTS.md`는 "AI 코딩 에이전트가 이 레포에 기여할 때 읽는
기여 가이드"입니다. 레포가 AI 도구인 것이 아니라, AI와 협업하기 좋게
정리된 레포입니다. README 배지에 `Powered by Anthropic`,
`Made with Codex`가 달려 있는 것은 이 레포 자체가 AI로 만들어졌다는
패러디의 일부입니다.

### 참고: 스킬로 래핑하면 가치가 올라갑니다

현재는 라이브러리이지만, Claude Code 스킬로 감싸면
"AI 스타트업 랜딩 만들어줘" 한 마디로 완성된 페이지를 생성할 수 있습니다.
수익화 아이디어 3번 참조.

## 8. API 토큰 필요 여부

결론: 필요 없습니다.

`fetch(`, `axios`, `api_key`, `process.env`, `XMLHttpRequest`, `websocket`
전체 검색 결과 네트워크 요청 코드가 0건입니다.
`anthropic` / `openai` 문자열이 걸리는 곳은 전부
`docs/lib/meta.tsx`의 "이 패턴을 쓰는 실제 회사" 참고 링크와
로고월 데모용 로고 이름뿐입니다.

| 항목 | 필요 여부 |
| --- | --- |
| OpenAI / Anthropic API 키 | 불필요 |
| 환경변수 (.env) | 불필요 |
| 서버 / 백엔드 | 불필요 (100% 프론트엔드) |
| 인터넷 연결 | 불필요 (오프라인 동작) |
| 사용료 | 없음 (MIT) |
| npm 토큰 | 직접 npm에 배포할 때만 필요 |

npm 토큰이 필요한 경우는 포크해서 자신의 패키지로 배포할 때뿐입니다.
`AGENTS.md`에 따르면 npm 2FA 또는 Classic Automation Token이 필요하고,
`E403`이 나면 `--otp=XXXXXX` 또는
`npm config set //registry.npmjs.org/:_authToken <token>`을 사용합니다.

### 중요한 전제

`TokenStream`, `ChatBubble`, `WibblingSpinner`는 모두 연출입니다.

- `TokenStream`은 `setTimeout`으로 미리 정해진 문자열을 쪼개어 보여줍니다.
- `WibblingSpinner`는 동사 186개를 랜덤 순환합니다. 실제 AI 상태와 무관합니다.

실제 AI 챗봇을 만들려면 UI는 이 라이브러리로, API 호출 로직은 직접
작성해야 합니다.

## 9. AI 에이전트 구축 활용성

UI 레이어에는 매우 유용하고, 로직 레이어에는 도움이 되지 않습니다.

### 활용 가능한 부분

| 필요한 UI | 컴포넌트 | 활용도 |
| --- | --- | --- |
| 사용자 입력창 | `Prompt`, `PromptHero` | 매우 높음 |
| 대화 말풍선 | `ChatBubble` (`ChatRole` 타입 제공) | 매우 높음 |
| 스트리밍 응답 | `TokenStream`, `useTokenStream` | 매우 높음 |
| 생각중 로딩 | `WibblingSpinner` | 매우 높음 |
| 코드 생성 결과 | `MockIDE` (`IdeToken`, `IdeTokenClass`) | 높음 |
| 모델 창의성 조절 | `Temperature` | 높음 |
| 에이전트 상태 | `StatusDot` | 높음 |
| 챗봇 띄우기 | `ChatFAB` | 높음 |
| 워크플로우 그래프 느낌 | `NodeGraphBackground` | 보통 |
| 토큰 / 호출수 카운터 | `StatCounter`, `useCounter` | 보통 |

### 실제 API와 결합하는 방법

헤드리스 훅 설계 덕분에 겉모습만 가져와 실제 스트리밍에 연결할 수 있습니다.

```tsx
function RealAgentBubble() {
  const [text, setText] = useState("");
  const [busy, setBusy] = useState(false);

  async function ask(prompt: string) {
    setBusy(true);
    const res = await fetch("/api/chat", {   // 서버 라우트 경유, 키는 서버 보관
      method: "POST",
      body: JSON.stringify({ prompt }),
    });
    const reader = res.body!.getReader();
    const dec = new TextDecoder();
    for (;;) {
      const { done, value } = await reader.read();
      if (done) break;
      setText((prev) => prev + dec.decode(value));
    }
    setBusy(false);
  }

  return (
    <>
      <Prompt onSubmit={ask} />
      <ChatBubble role="assistant">
        {busy && !text ? <WibblingSpinner /> : text}
      </ChatBubble>
    </>
  );
}
```

### 활용 불가능한 부분

- LLM 호출, 프롬프트 체이닝
- Tool calling / Function calling
- RAG, 벡터 DB, 임베딩
- 에이전트 루프, 메모리, 상태 머신
- MCP 통합

이 영역은 Anthropic SDK, Claude Agent SDK, Vercel AI SDK 등의 몫입니다.

### 권장 스택

```
화면 (UI)          performative-ui
상태 / 스트리밍     Vercel AI SDK (useChat)
에이전트 로직       Claude Agent SDK / Anthropic SDK
백엔드 (키 보관)    Next.js Route Handler / Express
```

API 키는 절대 프론트엔드에 넣지 않습니다. 브라우저 코드는 누구나 볼 수
있으므로 반드시 서버 라우트를 경유해야 합니다.

### 부가 가치: 에이전트 UX 인사이트

`research/` 문서들은 "AI 제품이 어떻게 AI처럼 보이는가"에 대한 연구입니다.

- 스트리밍 속도: 고정값보다 18~80ms 랜덤이 더 자연스럽게 느껴집니다
  (`useTokenStream`의 기본값이 `[18, 80]`입니다).
- 로딩 문구: 재미있는 동사를 랜덤 순환시키면 체감 대기시간이 줄어듭니다.
- 신뢰감: 상태 점, 토큰 카운터, 투명한 진행 표시가 기여합니다.

## 10. React / PHP 구현 가능성

### React

이미 React 라이브러리이므로 `npm install` 후 바로 사용합니다.

| 환경 | 호환성 | 비고 |
| --- | --- | --- |
| Vite + React | 완전 호환 | 레포 자체가 Vite 사용 |
| Create React App | 호환 | CSS import 확인 필요 |
| Next.js (App Router) | 주의 필요 | 아래 참조 |
| Remix / React Router v7 | 호환 | 문서 사이트가 RR7 사용 |
| Astro (React 아일랜드) | 호환 | `client:load` 지시어 필요 |
| React Native | 불가 | DOM / CSS 기반 |

Next.js App Router에서는 `useState` / `useEffect`를 쓰므로 클라이언트
컴포넌트로 선언해야 합니다.

```tsx
"use client";
import { Rotator } from "performative-ui";
```

CSS는 `app/layout.tsx`에서 import합니다. `AsciiHero`,
`NodeGraphBackground`는 Canvas를 쓰므로 하이드레이션 경고가 나면
`dynamic(() => import(...), { ssr: false })`로 감쌉니다.

직접 구현 연습 난이도 순서:

| 단계 | 컴포넌트 | 핵심 기술 |
| --- | --- | --- |
| 쉬움 | `GradientText` | `background-clip: text` |
| 쉬움 | `StatusDot` | div + `@keyframes` pulse |
| 쉬움 | `EyebrowPill` | `border-radius: 999px` |
| 쉬움 | `Sparkle` | 글리프 + 회전 애니메이션 |
| 보통 | `Rotator` | `useState` + `setTimeout` 타이핑 및 삭제 |
| 보통 | `StatCounter` | `requestAnimationFrame` 숫자 증가 |
| 보통 | `LogoMarquee` | `@keyframes translateX(-50%)` 무한 루프 |
| 보통 | `Aurora` | div 3개 + `filter: blur(120px)` |
| 어려움 | `NodeGraphBackground` | Canvas 2D, 점 N개 + 거리 기반 선 연결 |
| 어려움 | `AsciiHero` | Canvas 문자 그리드, 마우스 거리로 밝기 계산 |
| 어려움 | `Goldeneye` | `radial-gradient` 마스크 + 마우스 추적 |
| 어려움 | `Button` | 제네릭 forwardRef 폴리모픽 타이핑 |

### PHP

가능하지만 변환이 아니라 재작성입니다. 다만 생각보다 쉽습니다.

자산 구성 비율을 보면 실제 핵심은 CSS입니다.

```
styles.css       2,045줄   애니메이션, 글로우, 블러 전부 여기
components (33)  약 3,500줄  대부분 props를 className으로 바꾸는 얇은 래퍼
hooks (4)        약 300줄    타이머 로직
```

| 구분 | 난이도 | 방법 |
| --- | --- | --- |
| CSS (2,045줄) | 매우 쉬움 | 그대로 복사. 수정 0줄 |
| 정적 컴포넌트 (`GlassCard`, `LogoRow`, `PricingCard`, `BigBack`, `EyebrowPill`) | 쉬움 | PHP 함수로 HTML 출력 |
| 타이머 컴포넌트 (`Rotator`, `TokenStream`, `StatCounter`, `WibblingSpinner`) | 보통 | JS 필요. PHP는 서버 언어라 애니메이션 불가 |
| Canvas 컴포넌트 (`AsciiHero`, `NodeGraphBackground`) | 보통 | JS 필수. `demo/js/`에 바닐라 버전 존재 |

지름길은 `demo/` 폴더입니다. 최초 프로토타입이 바닐라 HTML / JS로
구현되어 있어 React 없이 동작하는 버전이 이미 존재합니다.

PHP 구현 예시:

```php
<?php
function pui_glass_card(string $html, string $extra = ''): string {
    $cls = trim("pui-glass $extra");
    return "<div class=\"$cls\">$html</div>";
}

function pui_button(string $label, string $variant = 'glow', bool $sparkle = false): string {
    $sp = $sparkle ? '<span class="pui-sparkle">&#10022;</span>' : '';
    return "<button class=\"pui-btn pui-btn--{$variant}\"><span>{$label}</span>{$sp}</button>";
}

function pui_rotator(array $words): string {
    $json = htmlspecialchars(json_encode($words, JSON_UNESCAPED_UNICODE), ENT_QUOTES);
    return "<span class=\"pui-rotator\" data-words='{$json}'></span>";
}
```

PHP 환경별 권장 방식:

| 환경 | 방법 |
| --- | --- |
| Laravel | Blade Component. `<x-pui-button variant="glow" sparkle />` |
| WordPress | 테마 또는 Gutenberg 블록. ThemeForest 판매 가능 |
| 순수 PHP | 함수 + include |
| Symfony (Twig) | Twig 매크로 또는 include |

### 미개척 영역

README 기준 커뮤니티 포트는 Svelte 2개뿐입니다.
Vue 3, WordPress, Laravel Blade, Angular, Web Components 포트는
아직 없으므로 선점 기회가 있습니다.

## 11. 유튜브 콘텐츠 제작 가능성

제작 가능하며, 소재 적합도가 매우 높습니다.

### 장점

1. 시각적 임팩트가 자동으로 확보됩니다 (글로우, 오로라, ASCII, 타이핑).
2. "AI 스타트업 디스"라는 유머 축이 있어 시청 지속시간이 유리합니다.
3. "요즘 사이트 다 똑같다"는 공감 포인트가 댓글 참여를 유도합니다.
4. 설치 3분 만에 결과가 나오므로 시청자 전환율이 높습니다.
5. MIT 라이선스로 법적 리스크가 낮습니다.

### 추천 콘텐츠 포맷

| 순위 | 포맷 | 길이 | 비고 |
| --- | --- | --- | --- |
| 1 | 쇼츠 시리즈 "AI 스타트업 홈페이지 클리셰 TOP 10" | 30~60초 × 10편 | research 문서가 대본 소재집 역할 |
| 2 | 라이브코딩 "30분 랜딩페이지 만들기" | 20~30분 | 완성본 먼저 보여주는 역순 훅 |
| 3 | 교육 시리즈 "React 라이브러리 저자처럼 코딩하기" | 10~15분 × 5편 | 개발자 타겟. 구독자 질 우수 |
| 4 | 챌린지 "라이브러리 없이 직접 만들기" | 15~20분 × N편 | 만든 후 원본 소스와 비교 |
| 5 | "AI와 함께 코딩" | 15~20분 | CLAUDE.md / AGENTS.md 설계 설명 |

### 제목 및 썸네일 아이디어

- "AI 스타트업 홈페이지, 왜 다 똑같이 생겼을까"
- "React 1줄로 AI 회사 홈페이지 만들기"
- "개발자가 AI 스타트업을 조롱하려고 만든 라이브러리"
- "30분 만에 유니콘처럼 보이는 랜딩페이지"

썸네일은 평범한 흰 페이지와 글로우 가득한 다크 페이지를 좌우로 배치한
비포애프터 대비가 효과적입니다.

### 주의사항

1. `research/` 문서는 특정 회사를 날카롭게 비꼽니다. 영상에서는 "이런
   패턴이 유행한다"로 일반화하고 개별 회사는 중립적으로 다룹니다.
2. MIT이지만 설명란에 원 레포 링크와 라이선스 표기를 넣는 것이 예의입니다.
3. 데모 로고월에 실제 기업 로고가 나옵니다. 영상에서는 가상 회사명으로
   교체하는 편이 안전합니다.
4. "실제 AI 기능은 없고 겉모습만 AI"라는 점을 명확히 짚어 시청자 오해를
   방지합니다.

## 12. 수익화 아이디어 7종

MIT 라이선스는 상업적 이용, 수정, 재배포, 유료 판매를 허용합니다.
단 저작권 고지와 라이선스 전문을 포함해야 하고, 보증이 없다는 점(AS-IS)을
고지해야 합니다. 핵심 원칙은 라이브러리를 파는 것이 아니라 그 위에 얹은
가치(템플릿, 강의, 도구, 서비스)를 파는 것입니다.

실제 기업 로고는 상표권 문제가 있으므로 상업 제품에서는 가상 회사명으로
교체합니다.

### 아이디어 1: 프리미엄 랜딩 템플릿 팩 판매

- 난이도: 낮음 / 초기투자: 거의 없음 / 가격: 개당 $19~79
- 상품 구성: 완성 템플릿 5종(에이전트 SaaS, 개발자 도구, 대기명단 전용,
  가격 중심, 포트폴리오), Next.js + Tailwind 세팅, 반응형, 다크/라이트,
  SEO/OG 메타, Figma 소스, 설치 영상, Vercel 1클릭 배포
- 실행: 1주 템플릿 2개 MVP → 2주 라이브 데모 배포 → 3주 Gumroad 또는
  Lemon Squeezy 등록 → 4주 ProductHunt, Reddit r/webdev, X, 디스콰이엇,
  GeekNews 홍보
- 예상: 월 방문 1,000명 × 전환 2% = 20건 × $39 = 약 $780/월
- 리스크: 무료 라이브러리와의 차별화 → "직접 조합 3일 vs $39"로 시간 가치를
  강조. 라이브 데모는 필수
- 팁: 무료 1종을 미끼로 배포하고 설치 가이드에서 유료 팩을 안내

### 아이디어 2: AI 랜딩페이지 초고속 제작 프리랜싱

- 난이도: 낮음 / 초기투자: 없음 / 가격: 건당 50~300만원
- 패키지: Lite 80만원(2일), Standard 180만원(4일), Pro 350만원(1.5주),
  유지보수 월 15만원
- 실행: 가상 회사명 포트폴리오 3개 제작 및 Vercel 배포 → 자기 소개
  페이지도 performative-ui로 제작 → 크몽, 숨고, 위시켓, Upwork, Fiverr,
  디스콰이엇, 로켓펀치 등록 → 최근 투자 유치 스타트업에 개선안 제안
- 예상: 월 2건 × 180만원 + 유지보수 5곳 × 15만원 = 약 435만원/월
- 리스크: "오픈소스 썼는데 왜 비싸냐"는 질문 → 견적서에 "검증된 컴포넌트
  활용으로 기간 1/5 단축"으로 명시. 디자인 중복은 `--pui-*` 토큰과 폰트
  커스텀으로 해소. 계약서에 수정 횟수 명시

### 아이디어 3: Claude Code 스킬 및 플러그인으로 패키징

- 난이도: 보통 / 초기투자: 없음 / 가격: 무료 배포 후 Pro $9/월 또는 $79
- 구조:

```
performative-landing-plugin/
  .claude-plugin/plugin.json
  skills/performative-landing/SKILL.md
  skills/performative-landing/references/catalog.md    컴포넌트 33개 + props
  skills/performative-landing/references/recipes.md    조합 레시피
  skills/performative-landing/references/theming.md    토큰 커스텀 가이드
  skills/ai-chat-ui/SKILL.md                           실제 API 연동 챗 UI
  commands/new-landing.md
  commands/pui-add.md
```

- 실행: 1주 `docs/lib/meta.tsx`(2,281줄)에서 컴포넌트명, props, 예제를
  추출해 `catalog.md` 작성 → 2주 레시피 4종 + SKILL.md → 3주 GitHub 무료
  공개 → 4주 이후 유료 Pro 버전
- 차별화: 한국어 지원과 한국형 레시피
- 부가 효과: 스킬 제작 경험 자체가 AI 에이전트 설계 학습 및 포트폴리오

### 아이디어 4: 미개척 프레임워크 포트 선점

- 난이도: 보통 / 초기투자: 없음 / 수익: 간접(인지도 → 수주 및 강의) 또는
  테마 판매

| 프레임워크 | 포트 존재 | 추천도 |
| --- | --- | --- |
| Vue 3 | 없음 | 매우 높음 |
| WordPress | 없음 | 매우 높음 (수익성 최상) |
| Laravel Blade | 없음 | 높음 |
| Angular | 없음 | 보통 |
| Astro 네이티브 | 없음 | 보통 |
| Web Components | 없음 | 높음 (프레임워크 무관) |

- WordPress 테마가 수익성이 가장 좋습니다. ThemeForest 기준 개당 $59~79,
  인기 테마는 누적 수천 건 이상 판매됩니다. 코딩을 하지 않는 창업자
  수요가 큽니다.
- 포팅 전략: `src/styles.css` 복사(수정 0줄) → `demo/js/*.js` 재활용 →
  정적 컴포넌트 15개(1주) → 애니메이션 컴포넌트 10개(1주) → Canvas
  컴포넌트 3개(3일). 약 3주면 1차 완성
- 예상: ThemeForest 월 20건 × $59 × 0.5 = 약 $590/월, 인기 테마 시 월
  $2,950
- 리스크: 유지보수 부담 → 인기 컴포넌트 15개로 범위 제한.
  원본 추적 → GitHub Watch 후 분기별 동기화

### 아이디어 5: 콘텐츠에서 강의 및 커뮤니티로

- 난이도: 보통 / 초기투자: 없음
- 깔때기: 유튜브(무료 유입) → Gumroad 템플릿 팩($39) → 인프런/유데미
  강의(7~8만원) → 유료 커뮤니티 멤버십(월 1만원) → 프리랜싱 수주
- 강의 커리큘럼 예시 (6시간, 79,000원):
  1. 왜 AI 홈페이지는 다 똑같은가 (research 활용, 30분)
  2. performative-ui 전체 투어와 랜딩 완성 (1시간)
  3. 직접 만들기: Rotator, StatCounter, LogoMarquee (1.5시간)
  4. 고급: 폴리모픽 컴포넌트와 헤드리스 훅 (1시간)
  5. Canvas 애니메이션 (1시간)
  6. npm 패키지 배포 (1시간)
- 예상: 유튜브 광고 월 30~80만원 + 강의 월 166만원 + 멤버십 50만원
- 리스크: 회사 비방 논란 → 패턴 일반화 필수. 성장에 3~6개월 소요 →
  쇼츠로 유입 먼저

### 아이디어 6: SaaS 랜딩페이지 빌더

- 난이도: 높음 / 초기투자: 중간 / 가격: $9~49/월
- 컨셉: 드래그앤드롭으로 블록 조립 후 즉시 배포
- 차별화: AI 스타트업 전용 초니치, AI 카피 자동 생성, 5분 완성,
  Webflow($23)보다 저렴한 $19
- 예상: 유료 100명 × $19 = $1,900/월 MRR, 500명 시 $9,500/월
- 리스크: Framer / Webflow와의 경쟁(높음), 개발 공수 3~6개월(높음)
- 권고: 아이디어 1, 2로 현금 흐름을 확보한 뒤 착수

### 아이디어 7: 실제로 작동하는 AI 챗 UI 키트

- 난이도: 보통 이상 / 초기투자: 없음 / 가격: $99~299 일회성
- 컨셉: performative-ui의 겉모습에 실제 API 연동을 붙인 유료 키트
- 구성: Anthropic(Claude) 및 OpenAI 스트리밍, 멀티턴 대화와 히스토리,
  실제 토큰 스트리밍(`useTokenStream` 개선판), Tool calling UI,
  파일 및 이미지 입력, 토큰 사용량과 비용 카운터, Temperature 실제 연동,
  에러 및 레이트리밋 처리, Next.js Route Handler(키 서버 보관),
  Vercel 1클릭 배포
- 근거: AI 앱 개발자가 급증하는데 챗 UI 구현(스트리밍, 스크롤, 중단,
  재생성, 에러 처리)은 의외로 까다롭습니다. `useTokenStream`의 빈
  의존성 배열 문제를 고쳐주는 것만으로도 가치가 있습니다
- 실행: 1주 스트리밍 기본 구현 → 2주 컴포넌트 연결 → 3주 히스토리와
  Tool calling UI → 4주 라이브 데모 및 Gumroad 등록
- 예상: 월 10건 × $149 + 엔터프라이즈 $499 × 2 = 약 $2,500/월

## 13. 추천 로드맵

| 기간 | 실행 | 목표 |
| --- | --- | --- |
| 1~2개월 | 아이디어 2(프리랜싱) + 1(템플릿) | 즉시 현금 흐름. 월 200만원 |
| 2~4개월 | 아이디어 3(스킬) + 5(유튜브) | 인지도와 브랜딩. 월 350만원 |
| 4~8개월 | 아이디어 4(WordPress 또는 Vue 포트) 또는 7(Agent UI Kit) | 패시브 인컴 자산화. 월 600만원 |
| 8개월 이후 | 아이디어 6(SaaS) | 자금 확보 후 스케일 도전 |

### 당장 실행할 항목

1. `npm install` 후 `npm run dev`로 컴포넌트 33개를 직접 확인
2. 마음에 드는 조합으로 랜딩 1개를 제작해 Vercel 배포
3. 해당 페이지를 가상 회사명으로 포트폴리오 1호로 저장
4. 크몽 또는 숨고에 "AI 스타트업 랜딩페이지 2일 제작" 서비스 등록
5. 쇼츠 1편 제작 ("AI 회사 홈페이지는 왜 다 똑같은가")

### 핵심 원칙

performative-ui를 파는 것이 아니라, performative-ui로 절약한 시간을
파는 것입니다. 구매자가 실제로 사는 것은 네 가지입니다.

- 시간: 3일에서 2시간으로 단축
- 판단: 33개 중 무엇을 어떻게 조합할지에 대한 결정
- 완성도: 반응형, SEO, 배포까지 포함한 마감
- 보증: 문제가 생겼을 때 물어볼 수 있는 사람

## 14. 참고 링크

| 항목 | 주소 |
| --- | --- |
| 작업 레포 (이 레포) | https://github.com/bmshin94/performativeUI |
| 원본 레포 | https://github.com/vorpus/performativeUI |
| 공식 문서 사이트 | https://performative-ui.cncl.co/ |
| npm 패키지 | https://www.npmjs.com/package/performative-ui |
| 이슈 트래커 | https://github.com/vorpus/performativeUI/issues |
| Svelte 커뮤니티 포트 1 | https://github.com/adv0r/performative-ui-svelte |
| Svelte 커뮤니티 포트 2 | https://github.com/benjamin-brady/performative-ui-svelte |
| Svelte 포트 2 라이브 문서 | https://benjamin-brady.github.io/performative-ui-svelte/ |
| 해외 소개 기사 (Gigazine, 일본어) | https://gigazine.net/news/20260609-performative-ui/ |
| 기여 가이드 | [AGENTS.md](./AGENTS.md) |
| 라이선스 | MIT. [README.md](./README.md) 참조 |

---

이 문서는 레포 컨벤션(`AGENTS.md`)에 따라 em dash와 이모지를 사용하지
않았습니다.
