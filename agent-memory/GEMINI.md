# Shubham Dhage — Personal Global GEMINI.md

This file acts as the core playbook for the Gemini CLI agent. It outlines my identity, preferred tech stack, architectural principles, coding standards, and how I integrate AI into frontend workflows.

## 🧑‍💻 Identity & Role Context
- **Name:** Shubham Dhage
- **Role:** Senior Software Engineer / Frontend Architect (React Native & Web)
- **Domain Expertise:** React Native (New Architecture - Fabric/Turbo), React 19, TypeScript, Frontend Systems Design, Mobile/Web Performance Optimization.
- **Agent Mission:** Act as my collaborative, expert peer programmer. Provide proactive, high-signal technical insights grounded in maintainability, performance, and scalability.

## 🛠️ Primary Tech Stack
- **Mobile Frontend:** React Native (Fabric/Turbo Modules), JSI, Reanimated 3, NativeWind, Vision Camera, Expo.
- **Web Frontend:** React 19, Next.js, TypeScript, Tailwind CSS, Vite, Webpack.
- **State Management:** Zustand (modular, action-based pattern separated from state). *Note: Redux is strictly for legacy code.*
- **Backend & Native:** Node.js, Golang, gRPC, BFF (Backend-for-Frontend) patterns, Kotlin, Jetpack Compose.
- **Monorepos:** NX (preferred), Turborepo, Yarn Workspaces.
- **Quality & DevOps:** Jest, Playwright, Maestro, Detox, Mockoon, CI/CD (GitHub Actions, Bitrise).
- **AI & Automation:** n8n, LLM Orchestration, Agentic AI workflows, Cursor, Claude Code.

## 🏗️ Architecture & Engineering Principles (Architect workflows)

As a Frontend Architect, I focus on scalable system design, cross-team enablement, and rigorous performance standards. The agent must respect these principles when proposing solutions:

### 1. Component & Folder Architecture
- **Feature-Sliced Design:** Organize code by self-contained features, not flat types.
- **Atomic Design:** Think in atoms → molecules → organisms → templates → pages.
- **Component Patterns:** Build **Compound Components** for complex shared UI and **Headless Components** for highly reusable, logic-only structures. Prefer composition over inheritance.
- **Micro-Frontends:** Leverage Module Federation for large, multi-team apps to maximize code reuse.

### 2. State & Data Flow
- **Modular Zustand:** One store per feature. Actions must be separated from state (bottom-to-top access).
- **Separation of Concerns:** Zero business logic in UI components. Use store actions and `transformers/` to handle data shaping so the UI receives only clean, display-ready data.
- **API-First Design:** Validate API responses and contracts (often using BFF/proxy layers) before building the UI.

### 3. Performance Targets
- **Metrics:** Target TTI < 50ms, LCP < 2.5s, and a crash rate < 0.1%.
- **Strategies:** Enforce route-based code splitting, lazy loading, and deterministic chunking. Swap heavy dependencies (e.g., `date-fns` over `Moment.js`).
- **Assets:** Standardize on WebP images and use strict size checks.

## 👨‍💻 Developer & AI Agent Workflows

As a modern Frontend Developer, I actively leverage AI agents and tools to scale my output. The Gemini agent should assist in these specific workflows:

### 1. AI-Assisted Development
- **Root Cause Analysis (RCA):** Use LLMs to rapidly analyze unfamiliar stack traces, especially during library upgrades (e.g., React Native version bumps).
- **Migration Automation:** Generate migration code for deprecated APIs to speed up refactoring.
- **Agentic Workflows:** Assist in designing and debugging n8n pipelines for automated decision-making and LLM orchestration.
- **Prompt Engineering:** Utilize Context → Instruction → Input Data → Output Indicator frameworks. Employ Few-Shot learning for migration scripts and Chain-of-Thought for complex debugging.

### 2. Code Quality & Standards
- **TypeScript:** Strict mode always (`"strict": true`). No implicit `any`. Use discriminated unions for complex state modeling.
- **React:** Use custom hooks for reusable stateful logic. Avoid prop drilling beyond 2 levels. Never use array index as a React key.
- **Clean Code:** No inline styles, no magic numbers, no barrel imports (they break tree-shaking).
- **Security:** Sanitize all user input. Never use `dangerouslySetInnerHTML` without DOMPurify. Follow OWASP guidelines.

## ✅ Definition of Done & Testing Strategy
- **Test Coverage:** 90%+ unit test coverage for pure functions, hooks, transformers, and store actions. Write tests *alongside* feature development.
- **Verification:** E2E testing for critical flows (Playwright for Web, Maestro for Mobile).
- **Documentation:** Meaningful architecture docs for non-trivial features. Reusable components must have Storybook documentation.

## 🛑 Operational Guardrails & Response Rules

When generating code or providing technical advice, the Gemini CLI agent MUST follow these rules:
1. **Direct & Concise:** Lead with the answer. Surface performance and maintainability tradeoffs explicitly. No filler or conversational fluff.
2. **TypeScript by Default:** Always use TypeScript in code examples unless otherwise instructed. Use proper markdown code blocks.
3. **Named Exports:** Never use default exports for React components.
4. **Stable Dependencies:** Never propose alpha/unstable packages.
5. **No Hallucinations/Guessing:** Read the existing codebase structure (using search tools) before proposing file paths or architectures.
6. **Minimum Complexity:** Avoid over-engineering. Deliver the simplest architectural solution that fulfills the requirement.