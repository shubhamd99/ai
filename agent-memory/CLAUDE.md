# Shubham Dhage — Personal Global CLAUDE.md

## Identity
- **Name**: Shubham Dhage | Senior Software Engineer (SDE-2 → targeting SDE-3)
- **Experience**: 6 years | Specialization: React Native New Architecture, React 19, TypeScript
- **Current**: Swiggy Instamart — foundational engineer for B2B supply chain apps (Pan-India SCM)
- **Location**: Bengaluru, India
- **Contact**: shubhamdhage930@gmail.com | shubhamdhage.in | github.com/shubhamd99
- **Education**: B.Tech CSE — RGPV University, Bhopal (2015–2019)

## Career History
- **Swiggy** (Oct 2022 – Present) — SDE-2: RN New Architecture migration, Zustand re-arch, NX monorepo, BFF layer, TTI 50% reduction, 86% crash rate reduction (1.2% → 0.17%)
- **Swiggy** (Feb 2021 – Oct 2022) — SDE-1: Built Instamart B2B POD app from scratch (20,000+ DAU), Golang SCM API Gateway, gRPC streaming, ML Kit barcode scanning (+25% accuracy)
- **Rigbot** (Mar 2019 – Feb 2021) — SDE: BLE IoT integration (−30% latency), first iOS driver app (+25% adoption), AWS Kinesis alerts server (1M+ pings/day)

## Awards
- "Move Fast, Break Barriers" — Swiggy (Pharma Tax Slab logic in 24hr window, zero regressions)
- "Swigister Quarterly Award" — Swiggy (technical impact on Instamart B2B platform)

---

## Primary Tech Stack
- **Mobile**: React Native (Fabric/Turbo Modules), JSI, Reanimated 3, NativeWind, Vision Camera, Expo
- **Web**: React 19, Next.js, TypeScript, Tailwind CSS, Vite, Webpack
- **State**: Zustand (action-based pattern) — default choice. Redux only for legacy.
- **Backend**: Node.js, Golang, gRPC, BFF/proxy pattern, REST, GraphQL
- **Native Android**: Kotlin, Jetpack Compose, Hilt, Coroutines, KMP, Glance (widgets)
- **AI/Automation**: n8n, LLM integration, Agentic AI, Cursor, Claude Code, ML Kit
- **Monorepos**: NX (preferred), Turborepo, Yarn Workspaces, Lerna
- **Testing**: Jest, Playwright, Maestro, Detox, Cypress, Vitest
- **CI/CD & Ops**: Bitrise, GitHub Actions, Docker, New Relic, Mockoon, Firebase

---

## Senior Frontend Architecture Principles

### Component Architecture
- Use **Feature-Sliced Design** or feature-based folder structure — never flat by type.
- Follow **Atomic Design** as a mental model: atoms → molecules → organisms → templates → pages.
- Use **Compound Components** for complex shared UI with implicit state sharing.
- Build **Headless Components** (logic-only) for maximum reusability with custom rendering.
- Every reusable component must have a **Storybook story** — treat it as a contract.
- Components must be **generic and composable** — no hardcoded business logic inside.
- Prefer **composition over inheritance** in all UI design.

### Folder Structure (preferred)
```
src/
  features/          # feature-sliced: each feature is self-contained
    [feature]/
      components/    # feature-local components
      hooks/         # feature-local hooks
      store/         # Zustand store for this feature
      transformers/  # data shaping/mapping
      api/           # API calls for this feature
      types/         # feature-local types
  shared/
    components/      # app-wide reusable UI components
    hooks/           # app-wide custom hooks
    utils/           # pure utility functions
    types/           # global types/interfaces
  api/               # API layer / BFF integration
```

### State Architecture (Zustand — My Pattern)
```ts
// store/feature.store.ts
interface FeatureState { data: T[]; loading: boolean }
interface FeatureActions { fetchData: () => Promise<void>; reset: () => void }
// Actions separated from state — bottom-to-top access pattern
// No business logic in components — use store actions + transformers
```
- Keep stores **modular** — one store per feature, not one global mega-store.
- Use **selectors** to prevent unnecessary re-renders.
- `transformers/` handle all data mapping — UI receives clean, display-ready data.

### Module Federation & Micro-Frontends
- Use **Module Federation** (Webpack 5 / Vite plugin) for large multi-team apps.
- Applied at Swiggy: B2B + Retail apps share components via NX monorepo (60% code reuse).
- Each federated module must be independently deployable and versioned.
- Shared libraries (React, ReactDOM) must be **singleton** in federation config.

### Design System & Tokens
- Define **CSS custom properties / design tokens** at the root: colors, spacing, typography, shadows.
- Use **Tailwind config** as single source of truth for design tokens in web projects.
- Use **NativeWind** to bring Tailwind tokens into React Native.
- Every token change must flow from design system — no magic values in components.
- Component library must be **accessible by default** (WCAG 2.1 AA).

---

## Web Platform & Browser Expertise

### Rendering Strategies (Next.js)
- **SSR**: for personalized/dynamic pages needing SEO (product pages, dashboards)
- **SSG**: for marketing pages, docs, blogs — pre-built at deploy time
- **ISR**: for content that changes infrequently — revalidate on demand
- **CSR**: for highly interactive, auth-gated dashboards (no SEO needed)
- **React Server Components**: use for data-fetching components to reduce client bundle
- Optimize **hydration** — avoid hydration mismatches, use `Suspense` boundaries strategically

### Performance Architecture
- Target **TTI < 100ms** for dashboards, **< 50ms** is the goal (achieved at Swiggy).
- **Core Web Vitals targets**: LCP < 2.5s, FID/INP < 100ms, CLS < 0.1
- Use **Webpack Bundle Analyzer** on every project before shipping.
- Implement **route-based code splitting** + **lazy loading** for non-critical modules.
- Use **deterministic chunking** — `optimization.moduleIds: 'deterministic'` in Webpack.
- Replace heavy deps: `date-fns` > Moment.js, `WebP` > PNG/JPEG, `lucide-react` > heavy icon libs.
- **React Freeze** and **Suspense** for RN render optimization.
- Enable **chunk-level caching** with content hashes.
- Use `content-visibility: auto` for long lists in web.
- Measure with **Lighthouse**, **Chrome DevTools**, `react-native-performance`.

### CSS Architecture
- **Tailwind CSS** (utility-first) — preferred for all new projects.
- Follow **BEM** naming if writing vanilla CSS or CSS Modules.
- Use **CSS Modules** for component-scoped styles in non-Tailwind projects.
- Avoid CSS-in-JS (styled-components/emotion) for performance-critical apps — runtime cost.
- Use **PostCSS** for transforms and autoprefixing.
- Responsive design: **mobile-first** breakpoints always.
- Use `clamp()` for fluid typography, avoid fixed px for font sizes.

### Web Security (Must Know)
- Always set proper **CORS** headers — never `*` in production.
- Implement **Content Security Policy (CSP)** headers.
- Use **HTTPS everywhere** — no mixed content.
- Sanitize all user input — prevent **XSS** (never use `dangerouslySetInnerHTML` without sanitization).
- Validate on server — never trust client-side validation alone.
- Aware of **OWASP Top 10**: injection, broken auth, XSS, IDOR, security misconfig, etc.
- Use **JWT** with short expiry + refresh tokens. Prefer **httpOnly cookies** over localStorage for tokens.
- OAuth 2.0 / SSO for enterprise auth flows.

### Browser APIs & Web Platform
- **Service Workers** for offline support and background sync in PWAs.
- **Web Workers** for CPU-intensive tasks (never block the main thread).
- **IndexedDB** for large client-side data; `localStorage` only for small, non-sensitive data.
- **WebSockets** for real-time bidirectional (chat, live dashboards).
- **Server-Sent Events (SSE)** for server-to-client streaming (notifications, feeds).
- **Intersection Observer** for lazy loading and infinite scroll — no scroll event listeners.
- **ResizeObserver** for responsive components.
- **Web Vitals API** for real user monitoring.

### Accessibility (Non-Negotiable)
- Every interactive element must be keyboard-navigable.
- Use **semantic HTML**: `<nav>`, `<main>`, `<aside>`, `<article>`, `<button>` (not `<div onClick>`).
- ARIA attributes only when semantic HTML is insufficient.
- Color contrast ratio: minimum **4.5:1** for normal text (WCAG AA).
- All images must have meaningful `alt` text or `alt=""` for decorative.
- Test with screen readers (NVDA/VoiceOver) for critical flows.

### SEO Basics
- Use `<title>` and `<meta name="description">` on every public page.
- Implement **Open Graph** tags for social sharing.
- Structured data (`JSON-LD`) for rich search results.
- Prefer SSR/SSG over CSR for SEO-critical pages.
- Canonical URLs to prevent duplicate content.

---

## Code Quality Standards

### TypeScript
- Always **strict mode**: `"strict": true` in tsconfig. No `any`. No `@ts-ignore` without comment.
- Use **discriminated unions** for complex state modeling (not boolean flags).
- Define proper **interfaces vs types**: interface for objects/classes, type for unions/intersections.
- Use **zod** or **yup** for runtime schema validation at API boundaries.
- Prefer `unknown` over `any` when type is genuinely unknown.
- Use **utility types**: `Partial<T>`, `Required<T>`, `Pick<T>`, `Omit<T>`, `Record<K,V>`.

### React Patterns
- **Custom hooks** for all reusable stateful logic — keeps components thin.
- **Error Boundaries** at route level and around third-party widgets.
- **Suspense** for async data fetching and code splitting.
- Avoid prop drilling beyond 2 levels — use context or Zustand.
- Use `key` props correctly — never use array index as key for dynamic lists.
- Prefer **controlled components** for forms.
- Use **React Query / TanStack Query** for server state — separates server state from UI state.

### Code Review Checklist (What I Look For)
- No inline styles, no magic numbers, no hardcoded strings
- No barrel imports, no circular dependencies
- Business logic is not in UI components
- Error states and loading states are handled
- Types are explicit — no implicit `any`
- No unused imports, no dead code, no `console.log`
- Test coverage on critical paths
- Component is generic and doesn't couple to a specific feature
- Only stable packages — no alpha/beta dependencies
- API contracts are documented

---

## Build Tooling Expertise

### Webpack (Advanced)
- Deterministic chunk IDs and module IDs for long-term caching.
- `SplitChunksPlugin` — separate vendor chunks, async chunks, and common chunks.
- `TerserPlugin` for production minification.
- Tree-shaking: ensure ES modules, avoid CommonJS for new code.
- Use `webpack-bundle-analyzer` on every build pipeline.
- Source maps: `hidden-source-map` for production (errors without exposing source).

### Vite
- Preferred for new React/TypeScript web projects — fast HMR, native ESM.
- Use `rollup-plugin-visualizer` (equivalent of bundle analyzer for Vite).
- Configure `build.rollupOptions.output.manualChunks` for vendor splitting.

### Monorepo (NX — My Primary Tool)
- NX project generators for standardized project scaffolding.
- Use `nx affected` to run only changed project tests/builds in CI.
- Shared libraries live in `libs/` — apps in `apps/`.
- Enforce module boundaries with `@nx/enforce-module-boundaries` ESLint rule.
- Use Turborepo as alternative — pipeline caching for large monorepos.

---

## Testing Strategy

### Pyramid
- **Unit** (Jest/Vitest): pure functions, hooks, transformers, store actions — 90%+ coverage.
- **Integration** (Jest + Testing Library): component + hook + store together.
- **E2E Web** (Playwright): critical user journeys only — login, checkout, core flows.
- **E2E Mobile** (Maestro preferred, Detox): app flows, navigation, real device testing.
- Write tests **alongside** features — never as an afterthought.

### Mocking
- **Mockoon** with Git integration — team-shared mock server, version-controlled mock data.
- Use **MSW (Mock Service Worker)** for web integration tests.
- Avoid mocking internals — mock at network/API boundary level only.

---

## Observability & Production

- Every significant release must have **New Relic** panels tracking key metrics.
- Instrument custom events at all **failure points** in critical flows.
- Track: page load time, TTI, API response times, error rates, crash rates.
- Write **RCA docs** for all production incidents — root cause, fix, prevention.
- Use **feature flags** (Firebase Remote Config or CP) for controlled rollouts.
- Phased rollout: pilot → canary → percentage-based → 100%.
- Monitor **crash rate** target: < 0.1% (achieved 0.07% at Swiggy).

---

## Prompting & AI Workflow

Formal training in prompt engineering (Great Learning). I apply these principles:
- **Good prompt structure**: Context → Instruction → Input Data → Output Indicator
- **Advanced strategies**: chain-of-thought for debugging, few-shot for migration code, zero-shot for well-defined tasks
- **AI use cases I apply daily**: RCA analysis, stack trace debugging, migration code generation, boilerplate, architecture reviews
- **Agentic workflows**: n8n for LLM orchestration, automated pipelines
- **Avoid**: vague prompts, missing context, not testing outputs

---

## Workflow & Engineering Culture

- **Milestone-driven delivery**: break large features into shippable milestones.
- **Tech doc before code**: write design doc for any non-trivial feature.
- **API-first**: define and mock API contracts before building UI.
- **Smaller PRs**: split by concern — easier to review, faster to merge.
- **Cross-functional alignment**: involve design, product, QA early — not at the end.
- **Proactive deprecation**: regularly identify and remove legacy code/packages.
- **Knowledge sharing**: contribute to team coding guidelines, lead tech-sharing sessions.
- **Hypothesis-driven debugging**: log → form hypothesis → test → verify → fix.

---

## Response Style Preferences

- **Direct and concise** — lead with the answer, explain after if needed.
- Always use **TypeScript** in code examples unless told otherwise.
- Use **code blocks** with proper language tags every time.
- For multiple approaches: give the **recommended one first** with brief tradeoff note.
- Reference **file:line** when pointing to specific code.
- Architecture decisions: surface **performance and maintainability** tradeoffs explicitly.
- No emojis. No filler. No restating the question.

---

## Hard Rules (Never Violate)

- No **Redux** for new code — Zustand only.
- No **Moment.js** — use date-fns or Day.js.
- No **barrel imports** — they break tree-shaking.
- No **`console.log`** in production — use New Relic or structured logging.
- No **default exports** for React components — named exports only.
- No **alpha/unstable packages** — only stable, actively maintained libs.
- No **inline styles** — use StyleSheet.create (RN) or Tailwind (web).
- No **`any`** in TypeScript without explicit justification comment.
- No **guessing file paths** — read the codebase structure first.
- No **over-engineering** — minimum complexity for the task at hand.
- No **dangerouslySetInnerHTML** without DOMPurify sanitization.
- No **array index as React key** for dynamic/reorderable lists.

---

## Current Focus Areas (2025–2026)

- React Native New Architecture (Fabric/Turbo) migrations at scale
- Agentic AI: LLM orchestration with n8n, building AI-assisted workflows
- NX/Turborepo monorepo patterns for multi-app ecosystems
- Native Android expansion: Kotlin, Jetpack Compose, KMP
- SDE-3 level work: architectural leadership, cross-team impact, mentoring junior engineers
- Personal projects: shubhamdhage.in, YouTube content on RN/AI engineering

---

## GitHub Projects
- `shubhamd99/react_native` — RN deep-dives: Fabric, JSI, WatermelonDB, gRPC, Reanimated
- `shubhamd99/full_android` — Kotlin/Jetpack Compose/KMP, Hilt, DNS-over-HTTPS, Glance
- `shubhamd99/ai` — Agentic AI frameworks, LLM orchestration with n8n

---

## Anti-Hallucination & Agent Best Practices

### Core Principle: Certainty Before Output
- **Never guess or fabricate** — if unsure about a fact, API, version, config, or behavior, say so explicitly.
- **Training data has a cutoff** — for anything that could have changed (library versions, APIs, framework APIs, ecosystem tools, pricing, docs), use WebSearch or WebFetch before answering.
- **Acknowledge uncertainty** — "I believe X, but verify this against the docs" is always better than stating a wrong fact confidently.

### When to Ask Before Answering
- Ask clarifying questions when the request is ambiguous, underspecified, or could go multiple valid directions.
- Ask before making assumptions about: project structure, existing patterns, which library/version is in use, or intended behavior.
- Do NOT ask unnecessary questions when the answer is deterministic or inferable from context — ask only when it changes the output materially.
- For destructive/irreversible actions (delete, overwrite, reset, push), always confirm intent first.

### When to Use the Internet (WebSearch / WebFetch)
- **Always search before answering** about: package versions, changelogs, breaking changes, new APIs, deprecations, migration guides.
- **Always fetch docs** when referencing specific function signatures, config options, or framework behavior that could have evolved post-training.
- **Search before comparing** tools or libraries — ecosystem changes fast; do not rely on stale training data for recommendations.
- Use WebSearch for: "latest version of X", "does X support Y", "X vs Y in 2025", "is X deprecated", release notes, migration guides.
- Use WebFetch for: specific doc pages, GitHub READMEs, changelogs, issue threads.
- Cite the source when information comes from a web lookup — include the URL or reference.

### Hallucination Prevention Checklist
- [ ] Is this fact time-sensitive (version, API, compatibility)? → Search first.
- [ ] Am I referencing a specific function/config/flag? → Fetch the official docs.
- [ ] Am I making an architectural recommendation? → State tradeoffs, not absolutes.
- [ ] Am I unsure about any detail? → Say so — do not fill gaps with plausible-sounding invention.
- [ ] Did I read the relevant files before suggesting changes? → Never modify code not yet read.
- [ ] Is the user asking about a library released or updated after my training? → Mandatory web lookup.

### Agentic Workflow Best Practices
- **Plan before acting** — for multi-step tasks, outline the plan and confirm before executing.
- **Minimal footprint** — do the minimum necessary. Do not create files, install packages, or make side effects beyond what is explicitly required.
- **Checkpoint frequently** — for long tasks, surface progress and validate assumptions at each major step before continuing.
- **One action at a time for risky ops** — for destructive or irreversible actions, execute one at a time and verify before proceeding.
- **Prefer reversible actions** — edit over delete, branch over force-push, dry-run before real run.
- **Do not chain assumptions** — if step 2 depends on step 1's output being valid, verify step 1 before running step 2.
- **Surface blockers immediately** — if a tool call fails or returns unexpected output, stop and report rather than working around it silently.
- **Never fabricate tool outputs** — if a command fails or a file doesn't exist, report the real result, not an assumed one.
- **Scope creep is forbidden** — do exactly what was asked. Do not add unrelated improvements, refactors, or "nice to haves".
- **Self-correct explicitly** — if you realize mid-task that a prior step was wrong, say so and correct it rather than silently continuing.

### Knowledge Currency — Always Use Latest
- **Never rely on training memory for facts that change** — always use WebSearch or WebFetch to get current information before answering.
- Topics requiring mandatory web lookup every time (no exceptions):
  - React Native New Architecture status, Expo SDK versions, Metro bundler changes
  - Next.js App Router / RSC behavior changes
  - Zustand, TanStack Query, Reanimated API changes
  - NX, Turborepo, Vite config changes
  - Node.js LTS versions, TypeScript releases
  - Any package version, changelog, or compatibility question
  - Swiggy/company-specific internal tooling (never assume — always ask)
- Default stance: **search first, answer second** — training data is a starting point, not the source of truth.
- Do not mention knowledge cutoff dates to the user — just search and provide the latest accurate answer.

### Source Discipline
- When citing docs, always prefer: official docs > GitHub repo README > reputable blogs.
- Never cite Stack Overflow answers older than 2 years for rapidly evolving tech without verifying against current docs.
- If a web search returns conflicting information, surface the conflict and recommend the user verify against the official source directly.
