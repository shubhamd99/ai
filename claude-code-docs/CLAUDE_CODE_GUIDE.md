# Claude Code — Complete Usage Guide

## Table of Contents
1. [Hooks](#hooks)
2. [Slash Commands](#slash-commands)
3. [MCP Plugins](#mcp-plugins)
4. [Settings & Config](#settings--config)
5. [Memory & CLAUDE.md](#memory--claudemd)
6. [Adding Directories](#adding-directories)
7. [Keyboard Shortcuts](#keyboard-shortcuts)
8. [CLI Flags](#cli-flags)
9. [Todos & Tasks](#todos--tasks)
10. [Vim Mode](#vim-mode)

---

## Hooks

Hooks let you run shell commands or HTTP requests at specific lifecycle events in Claude Code.

### Hook Event Types

| Event | When It Fires |
|-------|---------------|
| `SessionStart` | Session begins or resumes |
| `PreToolUse` | Before a tool executes |
| `PostToolUse` | After a tool succeeds |
| `PostToolUseFailure` | After a tool fails |
| `Notification` | When a notification is sent |
| `Stop` | When Claude finishes responding |
| `PreCompact` | Before context compaction |
| `SessionEnd` | Session terminates |

### Hook Handler Types

#### Command Hook
```json
{
  "type": "command",
  "command": ".claude/hooks/my-script.sh",
  "timeout": 30,
  "async": false
}
```

**Exit codes:**
- `0` — success, process stdout as JSON output
- `2` — blocking error, stderr shown to Claude as error message
- Other — non-blocking, stderr shown in verbose mode only

#### HTTP Hook
```json
{
  "type": "http",
  "url": "https://example.com/hook",
  "headers": { "Authorization": "Bearer $MY_TOKEN" },
  "allowedEnvVars": ["MY_TOKEN"],
  "timeout": 30
}
```

#### Prompt Hook (runs Claude to evaluate)
```json
{
  "type": "prompt",
  "prompt": "Should this tool call be allowed? $ARGUMENTS",
  "model": "fast-model",
  "timeout": 30
}
```

### Hook Configuration in settings.json

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": ".claude/hooks/validate-bash.sh"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": ".claude/hooks/format.sh"
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": ".claude/hooks/on-stop.sh"
          }
        ]
      }
    ]
  }
}
```

### Matcher Patterns

```json
{ "matcher": "Bash" }           // Exact match
{ "matcher": "Edit|Write" }     // Multiple tools (OR)
{ "matcher": "mcp__github__.*"} // All GitHub MCP tools (regex)
```

### Hook Input (via stdin)

All hooks receive JSON on stdin:
```json
{
  "session_id": "abc123",
  "hook_event_name": "PreToolUse",
  "cwd": "/current/working/directory",
  "tool_name": "Bash",
  "tool_input": { "command": "npm test" }
}
```

### Hook Output (via stdout)

Return JSON to influence behavior:
```json
{
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "allow",
    "permissionDecisionReason": "Safe command"
  }
}
```

### Hook File Locations

| Location | Scope |
|----------|-------|
| `~/.claude/settings.json` | Global (all projects) |
| `.claude/settings.json` | Project (git-shared) |
| `.claude/settings.local.json` | Project (personal, gitignored) |

---

## Slash Commands

Type `/` at the prompt to access commands with autocomplete.

### Session Management

| Command | Purpose |
|---------|---------|
| `/help` | Show help and all available commands |
| `/exit` | Exit Claude Code |
| `/clear` | Clear conversation history |
| `/compact [instructions]` | Compact conversation, free context |
| `/resume [session-id]` | Resume a previous session |
| `/fork [name]` | Fork conversation at current point |
| `/rewind` | Rewind code/conversation to previous checkpoint |
| `/rename [name]` | Rename current session |

### Configuration

| Command | Purpose |
|---------|---------|
| `/config` | Open settings UI — theme, model, output format |
| `/model [model-name]` | Change AI model |
| `/vim` | Toggle Vim editing mode |
| `/theme` | Change color theme |
| `/keybindings` | Create/open `~/.claude/keybindings.json` |
| `/terminal-setup` | Configure Shift+Enter and terminal keybindings |

### Memory & Context

| Command | Purpose |
|---------|---------|
| `/memory` | View/edit CLAUDE.md files and auto-memory |
| `/init` | Initialize project CLAUDE.md from codebase analysis |
| `/context` | Visualize context usage |
| `/add-dir <path>` | Add a directory to current session context |

### Code & Workflow

| Command | Purpose |
|---------|---------|
| `/diff` | Open interactive diff viewer for uncommitted changes |
| `/review` | Code review workflow |
| `/security-review` | Analyze changes for security issues |
| `/pr-comments [PR#]` | Fetch and display GitHub PR comments |
| `/install-github-app` | Set up Claude GitHub Actions integration |
| `/copy` | Copy last assistant response to clipboard |
| `/export [filename]` | Export conversation as plain text |

### Tools & Integrations

| Command | Purpose |
|---------|---------|
| `/mcp` | Manage MCP server connections and OAuth auth |
| `/hooks` | Manage hook configurations |
| `/plugin` | Manage Claude Code plugins |
| `/reload-plugins` | Reload plugins without restart |
| `/skills` | List available skills |
| `/ide` | Manage IDE integrations |
| `/chrome` | Configure Chrome browser integration |

### Usage & Diagnostics

| Command | Purpose |
|---------|---------|
| `/cost` | Show token usage statistics |
| `/usage` | Show plan limits and rate limit status |
| `/status` | Show version, model, account info |
| `/doctor` | Diagnose and verify installation |
| `/release-notes` | View changelog |
| `/feedback` | Submit feedback or bug report |

### Shortcuts

| Command | Purpose |
|---------|---------|
| `/todos` | See below — task management |
| `/tasks` | List and manage background tasks |
| `/fast [on\|off]` | Toggle fast mode |
| `/btw <question>` | Ask a quick side question (no tool use, low cost) |
| `/login` / `/logout` | Auth management |

---

## /todos and /tasks

### /todos

Displays the current list of TodoWrite tasks. When Claude Code uses the `TodoWrite` tool internally to plan multi-step work, `/todos` shows that list with completion status.

```
/todos
```

To manage tasks actively, ask Claude:
- `"show all tasks"`
- `"mark task 2 as done"`
- `"clear all tasks"`

Persist task list across context compactions:
```bash
CLAUDE_CODE_TASK_LIST_ID=my-project claude
```

### /tasks (Background Tasks)

Background tasks are long-running shell commands that don't block Claude:
```
Ctrl+B     — push current running task to background
Ctrl+T     — toggle background task list panel
/tasks     — list all background tasks with status
```

---

## MCP Plugins

MCP (Model Context Protocol) lets you connect Claude Code to external tools, APIs, and data sources.

### Adding MCP Servers

#### HTTP Server (remote API)
```bash
claude mcp add --transport http github https://api.githubcopilot.com/mcp/
```

#### Stdio Server (local process)
```bash
claude mcp add --transport stdio airtable \
  --env AIRTABLE_API_KEY=YOUR_KEY \
  -- npx -y airtable-mcp-server
```

On Windows, wrap with `cmd /c`:
```bash
claude mcp add --transport stdio my-server -- cmd /c npx -y @some/mcp-package
```

#### SSE Server (deprecated, but still supported)
```bash
claude mcp add --transport sse asana https://mcp.asana.com/sse
```

### MCP Management Commands

```bash
claude mcp list                    # List all configured servers
claude mcp get github              # Details for specific server
claude mcp remove github           # Remove a server
claude mcp add-from-claude-desktop # Import from Claude Desktop config
claude mcp reset-project-choices   # Reset project-level approval choices
claude mcp add-json name '{"type":"http","url":"..."}'
```

### Installation Scopes

| Scope | Location | Shared | Flag |
|-------|----------|--------|------|
| Local | `~/.claude.json` (per project) | No | `--scope local` (default) |
| Project | `.mcp.json` in project root | Yes (git) | `--scope project` |
| User | `~/.claude.json` (global) | No | `--scope user` |

### .mcp.json Format (project-level, git-shared)

```json
{
  "mcpServers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/"
    },
    "my-db": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@bytebase/dbhub"],
      "env": {
        "DB_URL": "postgresql://user:pass@host:5432/mydb"
      }
    }
  }
}
```

### Environment Variable Expansion in .mcp.json

```json
{
  "mcpServers": {
    "api": {
      "type": "http",
      "url": "${API_BASE_URL:-https://api.example.com}/mcp",
      "headers": {
        "Authorization": "Bearer ${API_KEY}"
      }
    }
  }
}
```

- `${VAR}` — expand env variable
- `${VAR:-default}` — expand with fallback

### OAuth Authentication for MCP

```bash
claude mcp add --transport http \
  --callback-port 8080 \
  my-server https://mcp.example.com/mcp
```

Then run `/mcp` inside Claude Code to complete OAuth flow.

### MCP Output Limits

```bash
MAX_MCP_OUTPUT_TOKENS=50000 claude   # Default: 25,000 tokens
```

### Using MCP Tools

Once an MCP server is connected, its tools appear as `mcp__<server>__<tool>`. Claude uses them automatically based on context. You can reference them in hooks:

```json
{ "matcher": "mcp__github__.*" }   // Hook on all GitHub MCP tools
```

---

## Settings & Config

### File Locations

| File | Scope |
|------|-------|
| `~/.claude/settings.json` | Global — all projects |
| `.claude/settings.json` | Project — git-shared |
| `.claude/settings.local.json` | Project — personal, gitignored |

**Precedence (highest → lowest):** Managed org policy → CLI flags → Local → Project → User global

### Open Settings UI

```
/config
```

Lets you change: model, theme, output format, verbose mode, vim mode.

### settings.json Example

```json
{
  "model": "claude-sonnet-4-6",
  "permissions": {
    "allow": [
      "Bash(npm run *)",
      "Bash(git *)",
      "Read",
      "Edit"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(curl *)",
      "Read(.env*)"
    ]
  },
  "env": {
    "NODE_ENV": "development",
    "API_URL": "http://localhost:3000"
  },
  "hooks": {},
  "autoMemoryEnabled": true
}
```

### Permissions Syntax

```json
"Bash(npm run *)"    // Allow bash commands matching pattern
"Bash"               // Allow all bash
"Read(.env*)"        // Deny reading .env files
"mcp__github__*"     // All GitHub MCP tools
```

---

## Memory & CLAUDE.md

### CLAUDE.md File Locations

| Location | Scope |
|----------|-------|
| `~/.claude/CLAUDE.md` | Global personal rules |
| `./CLAUDE.md` or `./.claude/CLAUDE.md` | Project team rules (git-shared) |
| `src/CLAUDE.md` | Subdirectory-specific rules (loaded on demand) |

### Creating Project CLAUDE.md

```
/init
```

Claude analyzes your codebase and auto-generates a starter `CLAUDE.md`.

### Writing Effective CLAUDE.md

```markdown
# Project: My App

## Stack
- React 19, TypeScript, Vite
- Zustand for state management
- Tailwind CSS

## Commands
- Dev: `npm run dev`
- Test: `npm test`
- Build: `npm run build`
- Lint: `npm run lint`

## Folder Structure
- `src/features/` — feature-sliced modules
- `src/shared/` — reusable components/hooks
- `src/api/` — API layer

## Coding Rules
- Named exports only — no default exports for components
- No `any` in TypeScript
- No `console.log` in production
- Use Zustand for state — no Redux
- Use `date-fns` — no Moment.js
```

**Tips:**
- Keep under 200 lines per file
- Be specific and verifiable ("2-space indent" not "format well")
- Use `@` to import other files: `@docs/api-guide.md`

### Auto Memory System

Stored in `~/.claude/projects/<project>/memory/`.

```
/memory
```

Opens interface to toggle auto-memory on/off and browse saved memory files.

Disable via settings:
```json
{ "autoMemoryEnabled": false }
```

Or env var:
```bash
export CLAUDE_CODE_DISABLE_AUTO_MEMORY=1
```

### Rules Directory (Advanced)

Place files in `.claude/rules/` for path-scoped rules:

```
.claude/
├── CLAUDE.md
└── rules/
    ├── code-style.md
    ├── testing.md
    └── api/
        └── validation.md
```

Rules with frontmatter load only when matching files are accessed:
```markdown
---
paths:
  - "src/api/**/*.ts"
---

# API Rules
- Validate all inputs with zod
```

---

## Adding Directories

### At Session Start (CLI)

```bash
claude --add-dir ../apps ../lib
claude --add-dir /absolute/path/to/shared
```

Paths are validated — must exist as directories.

### Load CLAUDE.md from Added Directories

```bash
CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1 claude --add-dir ../shared
```

### Mid-Session

```
/add-dir /path/to/directory
```

---

## Keyboard Shortcuts

### General

| Shortcut | Action |
|----------|--------|
| `Ctrl+C` | Cancel current input or generation |
| `Ctrl+D` | Exit Claude Code |
| `Ctrl+G` | Open in external editor |
| `Ctrl+L` | Clear terminal screen |
| `Ctrl+O` | Toggle verbose output |
| `Ctrl+R` | Search command history |
| `Ctrl+V` | Paste image from clipboard |
| `Ctrl+B` | Push running task to background |
| `Ctrl+T` | Toggle background task list |
| `Shift+Tab` | Cycle permission modes |
| `Alt+P` | Switch model |
| `Alt+T` | Toggle extended thinking |
| `Esc + Esc` | Rewind / summarize |

### Multiline Input

| Method | Shortcut |
|--------|----------|
| Default (macOS) | `Option+Enter` |
| iTerm2/WezTerm | `Shift+Enter` |
| Universal | `Ctrl+J` |
| Escape method | `\` + `Enter` |

### Text Editing

| Shortcut | Action |
|----------|--------|
| `Ctrl+K` | Delete to end of line |
| `Ctrl+U` | Delete entire line |
| `Ctrl+Y` | Paste deleted text |
| `Alt+B` | Move back one word |
| `Alt+F` | Move forward one word |

### Special Prefixes at Prompt

| Prefix | Meaning |
|--------|---------|
| `/` | Slash command or skill |
| `!` | Run as bash command directly |
| `@` | File path mention |
| `?` | Show available shortcuts |

### Customizing Keybindings

Run `/keybindings` to open `~/.claude/keybindings.json`:

```json
{
  "bindings": [
    {
      "context": "Chat",
      "bindings": {
        "ctrl+shift+enter": "chat:submit",
        "ctrl+e": "chat:externalEditor",
        "ctrl+u": null
      }
    }
  ]
}
```

**Contexts:** `Global`, `Chat`, `Autocomplete`, `Settings`, `Confirmation`, `Tabs`, `Help`, `Transcript`

**Key syntax:**
```
"ctrl+k"          — single modifier
"alt+shift+p"     — multiple modifiers
"ctrl+k ctrl+s"   — chord (sequential)
"escape"          — special key
```

---

## CLI Flags

### Basic Usage

```bash
claude                          # Start interactive session
claude "do something"           # Start with initial prompt
claude -p "query"               # Print response and exit (non-interactive)
claude -c                       # Continue most recent session
claude -r "session-id" "query"  # Resume specific session
```

### Model & Performance

```bash
claude --model claude-sonnet-4-6     # Set model
claude --model sonnet                # Use alias
claude --fast                        # Enable fast mode
claude --max-turns 5                 # Limit agentic turns
claude --max-budget-usd 5.00         # Spending limit
```

### Directories & Context

```bash
claude --add-dir ../apps ../lib      # Add working directories
```

### Permissions & Security

```bash
claude --permission-mode plan        # Start in plan mode
claude --permission-mode acceptEdits # Auto-accept edits
claude --tools "Bash,Edit,Read"      # Whitelist specific tools
claude --tools ""                    # Disable all tools
claude --allowedTools "Bash(npm *)"  # Pattern-based allow
claude --disallowedTools "Bash(rm *)" # Pattern-based deny
claude --dangerously-skip-permissions # Skip all permission checks
```

### Settings

```bash
claude --settings ./settings.json    # Load settings file
claude --system-prompt "You are..."  # Replace system prompt
claude --append-system-prompt "..."  # Append to system prompt
```

### MCP

```bash
claude --mcp-config ./mcp.json       # Load MCP config file
claude --strict-mcp-config           # Only use --mcp-config (ignore others)
```

### Output Format

```bash
claude -p "query" --output-format json          # JSON
claude -p "query" --output-format stream-json   # Streaming JSON
claude --verbose                                # Verbose logging
```

### Worktrees (Git Isolation)

```bash
claude --worktree feature-name       # Git worktree mode
claude -w feature-name               # Short form
```

---

## Vim Mode

Enable with `/vim` or in `/config`.

### Mode Reference (NORMAL mode commands)

**Navigation:**
```
h j k l     — left / down / up / right
w e b       — word forward / end / back
0 $         — line start / end
gg G        — input start / end
f{char}     — find char forward
```

**Editing:**
```
i a o       — insert before / after / new line below
I A O       — insert at line start / end / new line above
x           — delete character
dd          — delete line
dw de db    — delete word
cc          — change line
yy Y        — copy line
p P         — paste after / before
.           — repeat last change
```

**Text objects:**
```
iw aw       — inner / around word
i" a"       — inner / around quotes
i( a(       — inner / around parens
```

---

## Environment Variables Reference

```bash
CLAUDE_CODE_DISABLE_AUTO_MEMORY=1          # Disable auto memory
CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1  # Load CLAUDE.md from --add-dir paths
CLAUDE_CODE_TASK_LIST_ID=my-project        # Persist task list ID
CLAUDE_CODE_ENABLE_TELEMETRY=1             # Enable telemetry
ENABLE_TOOL_SEARCH=auto:5                  # Tool search threshold (% of context)
MAX_MCP_OUTPUT_TOKENS=50000               # MCP output token limit (default: 25000)
MCP_TIMEOUT=10000                          # MCP call timeout (ms)
MCP_CLIENT_SECRET=your-secret             # MCP OAuth client secret (CI use)
CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION=false # Disable prompt suggestions
```

---

## Quick Reference Card

```
/config        → open settings UI
/init          → create CLAUDE.md from codebase
/memory        → view/edit memory and CLAUDE.md files
/mcp           → manage MCP server connections
/add-dir       → add directory to session
/todos         → show current task list
/tasks         → background task management
/compact       → free up context window
/diff          → view uncommitted changes
/vim           → toggle vim editing mode
/keybindings   → customize keyboard shortcuts
/btw <q>       → quick side question (low cost)
! <cmd>        → run bash command directly
```
