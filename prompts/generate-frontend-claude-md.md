# Prompt: Generate Project-Level CLAUDE.md for a Frontend Repository

Use this prompt inside Claude Code (or any AI coding assistant) to generate a high-quality, production-grade `CLAUDE.md` file for any frontend project. Paste it as-is, or adjust the variables at the top before running.

---

## How to Use

1. Open Claude Code in your frontend project root.
2. Paste the prompt below (starting from the `---` divider).
3. Claude will analyze your codebase and generate a tailored `CLAUDE.md`.
4. Review and prune the output — remove anything Claude would already know from reading the code.

---

## The Prompt

```
Analyze this frontend repository and generate a production-grade CLAUDE.md file for it.

Before writing anything, do the following exploration steps:
1. Read package.json, package-lock.json or yarn.lock or pnpm-lock.yaml to identify the exact tech stack, framework version, and all key dependencies.
2. Read tsconfig.json or jsconfig.json if present.
3. Read the eslint config (.eslintrc.*, eslint.config.*), prettier config, and any stylelint config.
4. Read vite.config.*, next.config.*, webpack.config.*, or any other build config present.
5. Scan the src/ or app/ directory structure (one level deep) to understand folder organization.
6. Read any existing README.md, CONTRIBUTING.md, or docs/*.md files.
7. Look at 2–3 existing component files to understand naming conventions, export patterns, and coding style.
8. Look at any existing test files to understand the testing setup.
9. Check for .env.example or .env.local.example to understand required environment variables.
10. Check if there is an existing CLAUDE.md, AGENTS.md, or .cursorrules file to incorporate.

After exploration, generate a CLAUDE.md that follows ALL of these rules:

---

### CLAUDE.md Generation Rules

**Rule 1 — Only include what Claude cannot infer from the code.**
Do NOT include things Claude can figure out by reading files (e.g., "this project uses React" if it's obvious from imports). Only include non-obvious, project-specific facts, constraints, and decisions.

**Rule 2 — Keep it under 250 lines.**
Every line consumes context window on every session. Be ruthless. If removing a line would not cause Claude to make mistakes, remove it.

**Rule 3 — Use progressive disclosure.**
Do not inline long explanations. For anything detailed (architecture decisions, migration guides, complex workflows), create a reference file and link to it using @path/to/file syntax. The CLAUDE.md is an index, not a manual.

**Rule 4 — Be specific, not aspirational.**
Write "Use named exports only — no default exports for components" NOT "Write clean, readable code." Every instruction must be verifiable and actionable.

**Rule 5 — Emphasize critical rules.**
Mark rules that Claude frequently violates with "IMPORTANT:" or "NEVER:" prefix so they stand out.

**Rule 6 — Include only universally applicable content.**
Instructions that only apply to specific tasks belong in separate skill/agent files, not CLAUDE.md. Every line must be relevant to every session.

---

### Required Sections (include all that are applicable)

Generate CLAUDE.md with the following sections. Skip a section only if it is genuinely not applicable to this project.

#### 1. Project Overview (3–5 lines max)
- One sentence: what this project is and who uses it.
- The primary framework and rendering strategy (CSR / SSR / SSG / ISR).
- Any critical architectural constraint (e.g., "This is a micro-frontend shell — do not add business logic here").

#### 2. Tech Stack
List only the non-obvious or version-sensitive parts of the stack. Format as a compact table or short bullet list:
- Framework + version
- Language (TypeScript version, strict mode on/off)
- Styling approach (Tailwind / CSS Modules / styled-components / etc.)
- State management library
- Data fetching library (React Query, SWR, Apollo, etc.)
- Component library (shadcn/ui, Radix, MUI, etc.)
- Testing stack (Jest/Vitest + RTL, Playwright, Cypress, Maestro, etc.)
- Package manager (npm / yarn / pnpm / bun) — include exact command to use

#### 3. Essential Commands
Include exact, copy-pasteable commands Claude needs to verify its work. Group by purpose:

```markdown
## Commands
- Dev server: `<command>`
- Production build: `<command>`
- Type check: `<command>`
- Lint: `<command>`
- Format: `<command>`
- Run single test: `<command> <pattern>`
- Run all tests: `<command>`
- E2E tests: `<command>`
```

Only include commands that Claude can and should run. If a command is destructive or requires manual steps, note it.

#### 4. Project Structure
A brief map — only what is non-obvious or project-specific. Do NOT describe every folder. Focus on:
- Where feature code lives vs shared/common code
- Where API calls are made
- Where global state is defined
- Where types/interfaces are centralized
- Any non-standard folder names or unusual organization

Example format:
```
src/
  features/       # feature-sliced: each feature is self-contained
  shared/         # cross-feature components, hooks, utils
  api/            # all API layer code — never call APIs outside this folder
  store/          # global Zustand stores only — feature stores live in features/
```

#### 5. Code Style & Conventions
Only rules that differ from framework defaults or that Claude commonly gets wrong. Examples of what to include:
- Export pattern (named vs default)
- Import order (if enforced by linter rules)
- Naming conventions for files, components, hooks, constants
- Whether to use `interface` vs `type` for specific cases
- CSS class ordering (if Tailwind — mention if prettier-plugin-tailwindcss is configured)
- Any banned patterns (e.g., "No barrel files — they break tree-shaking", "No inline styles")
- Specific React patterns preferred (controlled vs uncontrolled, composition patterns, etc.)

#### 6. Architecture Decisions & Constraints
Document the WHY behind non-obvious architectural choices. Only include decisions Claude would otherwise second-guess:
- State management boundaries (what goes in Zustand vs React state vs URL state vs server state)
- Data fetching strategy (which layer fetches data, how loading/error states are handled)
- Routing patterns (file-based, layout nesting, protected routes implementation)
- Monorepo boundaries if applicable (what can import what)
- Performance constraints (bundle size limits, rendering rules, lazy loading requirements)

#### 7. Testing Conventions
- Preferred testing strategy (unit / integration / e2e boundaries)
- Where test files live (co-located or /tests directory)
- Mocking approach (MSW / vi.mock / jest.mock, what level to mock at)
- Any custom test utilities or render wrappers that must be used
- Coverage requirements if enforced

#### 8. Environment & Configuration
- Required environment variables (list names and purpose, NOT values)
- How to get local env variables set up (reference .env.example)
- Any environment-specific behavior Claude should know about
- Build-time vs runtime env var distinctions if relevant

#### 9. Non-Obvious Gotchas & Warnings
This is the highest-value section. Include things that would cause bugs or broken builds if Claude doesn't know them:
- Breaking constraints (e.g., "Server Components cannot use hooks — mark client components explicitly with 'use client'")
- Third-party library quirks (e.g., "shadcn components are copied into src/components/ui — do not edit the originals, re-generate with the CLI")
- Build quirks or CI constraints
- Known flaky tests or areas to avoid
- Authentication/authorization rules Claude must not break
- Database migration rules if applicable
- Any external service or API that has a sandbox vs production distinction

#### 10. Git & PR Conventions (if project-specific)
Only if the project has non-standard conventions:
- Branch naming pattern
- Commit message format (conventional commits, etc.)
- PR requirements (size limits, required reviewers, etc.)
- Changelog update requirements

#### 11. Progressive Disclosure References
At the bottom, list any detailed documentation files Claude should read on demand. Use @import syntax:
```markdown
## References (read on demand)
- Architecture deep-dive: @docs/architecture.md
- API integration guide: @docs/api-guide.md
- Design system tokens: @docs/design-tokens.md
- Deployment process: @docs/deployment.md
```

---

### Formatting Rules for the Output

- Use H2 (##) for section headers, H3 (###) for sub-sections.
- Use bullet lists for rules and short facts.
- Use code blocks with language tags for all commands and code examples.
- Use inline code (`backticks`) for file paths, command names, and specific values.
- Bold (**text**) for critical warnings or package names.
- Keep the entire file under 250 lines.
- Do NOT use emojis.
- Do NOT write introductory or closing filler sentences.
- Start the file with the project name as an H1 heading.

---

### Anti-Patterns to Avoid in the Generated CLAUDE.md

Do NOT include any of the following — they waste context and degrade instruction-following quality:

- Self-evident standards: "Write clean code", "Use meaningful variable names", "Follow best practices"
- Standard language conventions Claude already knows: "TypeScript supports interfaces", "React components must return JSX"
- Redundant tool documentation: Do not explain what Tailwind or React Query does
- File-by-file codebase tour: Claude can read files directly
- Long tutorials or explanations: Reference a doc file instead
- Instructions that only apply to one specific task
- Placeholder or aspirational content without concrete rules
- Anything that would be in a standard README
- Code style rules already enforced by the ESLint/Prettier config — Claude will follow linter output

---

### Final Check Before Outputting

Before writing the final CLAUDE.md, ask yourself these questions for each line:
1. Would Claude make a mistake without this instruction? (If no → delete it)
2. Can Claude infer this from reading the code? (If yes → delete it)
3. Is this universally applicable to every session, not just specific tasks? (If no → move to a referenced doc)
4. Is this specific and verifiable? (If vague → rewrite or delete)
5. Is the total under 250 lines? (If no → prune further)

Output the CLAUDE.md file directly. Do not include any explanation or commentary outside the file content.
```

---

## Output Quality Checklist

After Claude generates the CLAUDE.md, validate it against these criteria:

### Structure
- [ ] Starts with project name as H1
- [ ] Has all applicable sections from the template above
- [ ] Under 250 lines
- [ ] No emojis, no filler sentences, no vague aspirational rules

### Content Quality
- [ ] Every rule is specific and actionable, not aspirational
- [ ] Critical rules are marked with IMPORTANT: or NEVER:
- [ ] Commands are exact and copy-pasteable
- [ ] No standard conventions Claude already knows
- [ ] Gotchas section includes the real footguns of this specific project

### Context Efficiency
- [ ] Long docs are referenced via @path not inlined
- [ ] No file-by-file codebase tour
- [ ] No ESLint/Prettier rules that the linter enforces automatically
- [ ] Nothing that would only apply to a single specific task

### Team Value
- [ ] Can be committed to git and shared with the full team
- [ ] Gives a new engineer (or AI) the same mental model as an experienced team member
- [ ] Would be useful for a developer onboarding to this project

---

## Maintenance Tips

- **Treat CLAUDE.md like code.** Review it in PRs, prune dead rules, add new gotchas as you discover them.
- **When Claude repeatedly ignores a rule**, the file is too long — prune unrelated rules to give that rule more signal.
- **When Claude asks questions answered in CLAUDE.md**, the phrasing is ambiguous — rewrite that section more concisely and directly.
- **Use IMPORTANT: / NEVER: prefix** for rules Claude gets wrong frequently.
- **Do not auto-regenerate CLAUDE.md** — every line has leverage across every session. Manually curate the contents.
- **For monorepos**, create sub-directory CLAUDE.md files in `apps/frontend/`, `packages/ui/` etc. with context specific to that package. The root CLAUDE.md should only contain cross-cutting concerns.
- **Add to `.claude/rules/`** for path-scoped rules — e.g., API validation rules only when editing `src/api/**/*.ts`.

---

## Example Snippet (What Good Output Looks Like)

```markdown
# Acme Dashboard

B2B analytics dashboard built on Next.js App Router with SSR for authenticated routes and SSG for public marketing pages.

## Tech Stack
- **Framework**: Next.js 15 (App Router) — TypeScript strict mode
- **Styling**: Tailwind CSS 4 + `prettier-plugin-tailwindcss` (auto class ordering)
- **Components**: shadcn/ui — components are in `src/components/ui/`, generated via CLI, do not hand-edit
- **State**: Zustand (feature-scoped stores) + TanStack Query v5 (server state)
- **Auth**: NextAuth.js v5 — session accessed via `auth()` server-side, `useSession()` client-side
- **Package manager**: pnpm — use `pnpm` not `npm` or `yarn`

## Commands
- Dev: `pnpm dev`
- Build: `pnpm build`
- Type check: `pnpm tsc --noEmit`
- Lint: `pnpm lint`
- Single test: `pnpm vitest run src/features/billing`
- All tests: `pnpm test`
- E2E: `pnpm playwright test`

## Project Structure
```
src/
  app/              # Next.js App Router pages and layouts
  features/         # Feature-sliced: each feature owns components, hooks, store, api
  shared/           # Cross-feature: ui primitives, hooks, utils
  server/           # Server-only code: DB queries, auth helpers
  api/              # API route handlers only — no business logic here
```

## Code Conventions
- **Named exports only** — NEVER default exports for components or hooks
- **No barrel files** — import directly from the source file
- Components: PascalCase files (`UserCard.tsx`), hooks: camelCase with `use` prefix (`useUserProfile.ts`)
- Use `type` for props/data shapes, `interface` for extendable public contracts
- Server Components by default; add `'use client'` only when hooks or browser APIs are needed

## Architecture Constraints
- IMPORTANT: Never call `fetch` or `axios` directly in components — use TanStack Query with hooks from `src/features/*/api/`
- IMPORTANT: Never put server-only code (DB, env secrets) in files without `.server.ts` suffix or `server/` directory
- Route protection: middleware handles auth redirect — do not add per-page auth checks
- Feature stores: one Zustand store per feature in `src/features/*/store.ts` — no global mega-store

## Environment Variables
Required in `.env.local` (see `.env.example`):
- `DATABASE_URL` — Postgres connection string
- `NEXTAUTH_SECRET` — Random secret for NextAuth
- `NEXTAUTH_URL` — App base URL

## Gotchas
- NEVER run `pnpm dlx shadcn@latest add` without reviewing what it overwrites — it modifies existing files
- The `src/server/db.ts` Prisma client is a singleton — do not instantiate `new PrismaClient()` elsewhere
- `app/api/` routes run on Edge Runtime — do not use Node.js APIs there
- Charts use Recharts v2 — not v3; the API changed significantly between versions
```

---

## Related Resources

- [Official Claude Code Best Practices](https://code.claude.com/docs/en/best-practices)
- [Writing a Good CLAUDE.md — HumanLayer](https://www.humanlayer.dev/blog/writing-a-good-claude-md)
- [How to Write a Good CLAUDE.md — Builder.io](https://www.builder.io/blog/claude-md-guide)
- [Next.js Official: AI Coding Agents Guide](https://nextjs.org/docs/app/guides/ai-agents)
- [CLAUDE.md Example: Next.js + shadcn + React Query](https://gist.github.com/gregsantos/2fc7d7551631b809efa18a0bc4debd2a)
