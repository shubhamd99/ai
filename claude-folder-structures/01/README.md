# 🤖 The .claude Folder Setup Guide

> **Project Brain & AI Agent Orchestration**
> Transform your development workflow by turning Claude into a modular, specialist-driven engineering team.

Based on the [LeadGenMan Guide](https://resources.leadgenman.com/ClaudeFolderSetup), this architecture allows for fine-grained control over AI agents, custom commands, and automated hooks within your local environment.

---

## 🏗️ The Structure

The core of this setup lives within the `.claude/` directory at your project root, alongside a `CLAUDE.md` project context file.

```text
.claude/
├── agents/          ← specialist AI team members (e.g., code-reviewer, debugger)
├── commands/        ← custom /slash commands for automated workflows
├── hooks/           ← mandatory scripts Claude MUST execute (pre/post tool)
├── rules/           ← context-aware instructions loaded based on file paths
├── skills/          ← situational intelligence & design standards
└── settings.json    ← Permission and model configuration control center
CLAUDE.md            ← The project brain (tech stack, conventions, and status)
```

---

## ⚙️ Core Components

### 1. Agents (`.claude/agents/*.md`)
Specialists that Claude delegates complex tasks to. Each agent has a specific focus and toolset.
- **Common Agents:** `code-reviewer`, `debugger`, `test-writer`, `refactorer`, `doc-writer`, `security-auditor`.
- **Example:** A `code-reviewer.md` might use `sonnet` with access to `Bash` and `Grep` to perform automated security scans.

### 2. Commands (`.claude/commands/*.md`)
Reusable prompt templates triggered by slash commands.
- **Example:** `/fix-issue 42` triggers a workflow to view the GitHub issue, implement a fix, and run regression tests.

### 3. Hooks (`.claude/hooks/*.sh`)
Automated scripts that run on tool usage or events. Use exit code `2` to block an action and `0` to allow.
- **Use Cases:** Running `tsc` or `lint` before a commit, or auto-formatting on save.

### 4. Rules (`.claude/rules/*.md`)
Scoped instructions that load only when you touch specific files (configured via the `paths:` YAML header).
- **Example:** `frontend.md` loads when editing `.tsx` files to enforce `shadcn/ui` and `Tailwind` standards.

### 5. Skills (`.claude/skills/*.md`)
Deep situational awareness, such as specific design palettes or complex architectural patterns.
- **Example:** `frontend-design/SKILL.md` can define exact spacing, radii, and color tokens.

### 6. Project Brain (`CLAUDE.md`)
The essential project summary loaded every session.
- **Stack:** Define your exact tech stack (e.g., Next.js 15, React 19, Zustand).
- **Conventions:** Strict formatting, testing rules, and naming conventions.

---

## 🚀 Quick Setup

Run the following command in your terminal to initialize the architecture:

```bash
mkdir -p .claude/{agents,commands,hooks,rules,skills/frontend-design} && \
touch .claude/agents/{code-reviewer,debugger,test-writer,refactorer,doc-writer,security-auditor}.md && \
touch .claude/commands/{fix-issue,deploy,pr-review}.md && \
touch .claude/hooks/{pre-commit,lint-on-save}.sh && \
touch .claude/rules/{frontend,database,api}.md && \
touch .claude/skills/frontend-design/SKILL.md && \
touch .claude/settings.json CLAUDE.md && \
chmod +x .claude/hooks/*.sh
```

---

## 🎯 Key Benefits
- **Specialization:** No more "jack-of-all-trades" hallucinations.
- **Automation:** Reduce repetitive setup tasks via slash commands.
- **Consistency:** Mandatory hooks ensure high-quality code standards.
- **Performance:** Scoped rules keep the context window clean and focused.

---
*Summary generated for @ShubhamDhage based on the LeadGenMan framework.*
