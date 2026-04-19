---
name: agents-setup
description: >
  Sets up or repairs the AGENTS.md source-of-truth pattern for any project.
  Creates a well-structured AGENTS.md with real stack info auto-detected from
  the project, then wires all AI config satellites (.claude/CLAUDE.md,
  .github/copilot-instructions.md, .agents/rules/, MEMORY.md) to point to it.
  Eliminates duplication. Always runs in plan mode — asks before acting.
  Use this skill whenever the user mentions AGENTS.md, agent config, source of
  truth for AI rules, setting up Claude/Copilot/Cursor for a project, fixing
  duplicate AI instructions, or wants to consolidate AI configuration files.
  Trigger even if the user just says "set up agents" or "fix my AI config".
---

# agents-setup

Consolidate AI agent configuration into a single AGENTS.md source of truth.
Wire all satellite config files to point to it. Eliminate duplication.

## Communication

Always respond in caveman mode (full intensity):

- Drop articles, filler, pleasantries, hedging
- Fragments OK, short synonyms, technical terms exact
- Code blocks unchanged
- Safety override: clear wording for destructive ops, then resume caveman

## Phase 1: Plan Mode

**Always start here. Never skip.**

Open with exactly this:

> "Modo plan: Preguntame todo lo que necesites para realizar correctamente esta tarea."

Then ask only what's missing. Scan the project first (see Detection below), then ask only the gaps you can't auto-detect:

- Project name / purpose (if not obvious from package.json or README)
- LLMs/tools in use beyond Claude Code (Cursor? Copilot? OpenCode? Zed?)
- Any existing rules in current AI config files worth preserving?
- Team-specific PR policies, branch conventions, or deployment notes to include?

Present a plan listing every file you'll create or modify. Wait for explicit approval ("go", "sí", "proceed", "yes") before touching anything.

## Phase 2: Auto-Detection

Scan the project to build an accurate picture of the stack. Do this before asking questions — the answers shape what you ask.

### Detect stack

Read in this order:

1. **Root package.json** — name, scripts, dependencies, devDependencies
2. **Workspace package.json files** — monorepo packages (look for `workspaces` field or multiple `package.json` in subdirs)
3. **Other stack indicators**: `pyproject.toml` / `requirements.txt` (Python), `go.mod` (Go), `Cargo.toml` (Rust), `pom.xml` / `build.gradle` (Java), `Gemfile` (Ruby), `composer.json` (PHP)
4. **Build tools**: vite.config._, webpack.config._, turbo.json, nx.json
5. **Test framework**: jest.config._, vitest.config._, pytest.ini, .mocharc.\*
6. **Lint/format**: .eslintrc._, .prettierrc._, pyproject.toml [tool.ruff], etc.
7. **Package manager**: presence of bun.lock, pnpm-lock.yaml, yarn.lock, package-lock.json

### Detect existing AI config

Check for these files and read their contents:

- `AGENTS.md`
- `.claude/CLAUDE.md`
- `.github/copilot-instructions.md`
- `.cursorrules`
- `.windsurfrules`
- `.agents/rules/*.md` (list all)
- `MEMORY.md`

Flag any rules in satellite files that are NOT already covered by AGENTS.md — these need preserving.

### Detect project structure

List top-level directories (excluding node_modules, .git, dist, build, .cache).
For monorepos: list each package and its role if inferable.

## Phase 3: Generate AGENTS.md

Create or update AGENTS.md at project root. Keep under 200 lines.

### Template

```markdown
# AGENTS.md — Project Rules

## Stack

[Fill from auto-detection. Be specific: versions, framework names, key libraries.]
[For monorepos: list each package and its role.]

## Commands

[Extract real commands from package.json scripts or equivalent.]
[Group by: dev, build, test, lint/format, any project-specific workflows.]

## Project Structure

[List top-level dirs with one-line purpose each.]
[For monorepos: list packages + roles.]

## Communication Style

Default mode for all AI assistants in this project: **caveman** (full intensity).

Rules:

- Load `/caveman` skill (level: full) before any response
- Apply to: chat replies, planning steps, tool preambles, progress updates, summaries
- Disable only on explicit: `stop caveman` or `normal mode`
- Fallback if skill unavailable: emulate caveman style directly
- Safety override: use clear wording for destructive/irreversible ops, then resume caveman

## Plan Before Act

Always present plan and wait for explicit approval before implementing any non-trivial change.

Non-trivial = anything beyond single-line fix. Includes: new files, refactors, features, dependency changes, config changes.

Format:

1. State what you found
2. List proposed changes (files + what changes)
3. Flag risks or unknowns
4. Wait for "go" / "yes" / "proceed"

Do NOT write code until approved.

## PR Policy

[Fill from project conventions or use sensible defaults:]

- One concern per PR
- Run lint + format before committing
- Tests required for new logic
- No TODO left in merged code
```

### Rules for filling the template

- **Stack**: exact versions from package.json, not ranges (React 19, not ^19). For monorepos list each package's role.
- **Commands**: use actual script names from package.json. If `bun run dev` is the real command, write that — not `npm run dev`.
- **Structure**: only include dirs that exist. Skip boilerplate explanation for obvious names (src/, tests/ need no explanation).
- **Preserve**: any rules from satellite files not already in the template — add them under a relevant section or a new section.
- **Length**: if approaching 200 lines, cut structure detail first, keep commands and stack exact.

## Phase 4: Wire Satellite Files

After AGENTS.md is ready, update each satellite:

### .claude/CLAUDE.md

If file exists: replace entire content with one line.
If file doesn't exist: create it.

```
Strictly follow the rules in ./AGENTS.md
```

### .github/copilot-instructions.md

Same — one line:

```
Strictly follow the rules in ./AGENTS.md
```

### .cursorrules (only if file already exists)

Replace with:

```
Strictly follow the rules in ./AGENTS.md
```

### .windsurfrules (only if file already exists)

Replace with:

```
Strictly follow the rules in ./AGENTS.md
```

### .agents/rules/ (if directory exists)

List all .md files. For each:

- Read content
- If content is fully covered by AGENTS.md → delete file
- If content has rules NOT in AGENTS.md → port those rules to AGENTS.md, then delete file
- Tell user which files were deleted and what (if anything) was ported

### MEMORY.md

If file doesn't exist, create at project root:

```markdown
# MEMORY.md — Agent Lessons

Human-reviewed lessons from past sessions. Agent writes candidates; human approves before merging.

---

<!-- Add lessons below. Format: date | lesson | why it matters -->
```

If file exists, leave it untouched.

## Phase 5: Report

After all changes, report:

```
Done. Changes:
- AGENTS.md: [created/updated]
- .claude/CLAUDE.md: [created/updated] → pointer
- .github/copilot-instructions.md: [created/updated] → pointer
- .agents/rules/: [N files deleted, M rules ported to AGENTS.md]
- MEMORY.md: [created/already existed]
- [any other files touched]

Single source of truth: AGENTS.md
```

Then ask: "Hay algo que ajustar en AGENTS.md antes de cerrar?"
