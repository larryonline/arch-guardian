# Arch Guardian

A Claude Code skill for structured architecture review — preventing architecture degradation through systematic inspection.

[中文文档](README_CN.md)

## Quick Install

```bash
claude install-skill https://github.com/larryonline/arch-guardian/raw/main/arch-guardian.skill
```

## What It Does

Arch Guardian guides Claude through a structured architecture review of your project, checking three layers:

| Layer | Checks |
|-------|--------|
| **Architecture** | Layer clarity, directory structure, global dependencies, change impact |
| **Module** | Module boundaries, inter-module coupling, replaceability, data flow |
| **Component** | Single responsibility, naming readability, dependency direction, extensibility |

It supports two review modes:

- **Design Review (pre-implementation)** — Assess how a proposed feature/module impacts overall architecture before writing code
- **Implementation Review (post-implementation)** — Verify that existing code conforms to architectural conventions

## Usage

Once installed, the skill triggers automatically when you mention architecture-related topics in Claude Code:

- "Review the architecture impact of this change"
- "The project structure feels messy, help me sort it out"
- "I'm adding a new module, let's do a design review first"

## How It Works

1. **Knowledge Discovery** — On first use, scans your project for architecture docs, conventions, tech stack info, and ADRs
2. **Scoped Review** — Adjusts review depth (focused / standard / comprehensive) based on change scope
3. **Structured Report** — Outputs a report with pass/warn/fail verdicts for each check item, with specific findings and actionable suggestions
4. **Persistent History** — Saves reports to `.claude/arch-guardian/` for tracking architecture evolution over time

## License

MIT
