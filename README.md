# Arch Guardian

A Claude Code skill for structured architecture review — preventing architecture degradation through systematic inspection.

[中文文档](README_CN.md)

## Quick Install

**Option 1 — Paste into Claude Code chat:**

```
Install arch-guardian from https://raw.githubusercontent.com/larryonline/arch-guardian/main/install.md
```

**Option 2 — Run in terminal:**

```bash
claude "Install arch-guardian from https://raw.githubusercontent.com/larryonline/arch-guardian/main/install.md"
```

Claude will guide you through the setup interactively — asking where to install and whether to configure the pre-commit hook.

## What It Does

Arch Guardian supports two modes:

**Manual review** — triggered when you mention architecture-related topics in Claude Code. Guides Claude through a structured three-layer inspection:

| Layer | Checks |
|-------|--------|
| **Architecture** | Layer clarity, directory structure, global dependencies, change impact |
| **Module** | Module boundaries, inter-module coupling, replaceability, data flow |
| **Component** | Single responsibility, naming readability, dependency direction, extensibility |

**Change impact speed check** — triggered automatically before `git commit`. Performs a lightweight scan of staged changes across four dimensions (layer assignment, cross-layer dependencies, module boundaries, interface changes) and outputs a one-line notice only when something needs attention. Silent on safe changes.

## Usage

**Manual review** — mention architecture-related topics:

- "Review the architecture impact of this change"
- "The project structure feels messy, help me sort it out"
- "I'm adding a new module, let's do a design review first"

**Speed check** — happens automatically before every `git commit`. On first use, Arch Guardian will offer to configure the hook for you.

## How It Works

1. **Hook setup** — On first use, detects if the pre-commit hook is configured and offers to set it up automatically
2. **Knowledge Discovery** — Scans your project for architecture docs, conventions, tech stack info, and ADRs; persists findings to `.claude/arch-guardian/guardian-knowledge.md`
3. **Scoped Review** — Adjusts review depth (focused / standard / comprehensive) based on change scope
4. **Structured Report** — Outputs pass/warn/fail verdicts for each check item with specific findings and actionable suggestions
5. **Persistent History** — Saves reports to `.claude/arch-guardian/` for tracking architecture evolution over time

## License

MIT
