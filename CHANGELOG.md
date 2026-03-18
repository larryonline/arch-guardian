# Changelog

## v0.0.2

- Added change impact speed check mode, triggered automatically before `git commit` via Claude Code PreToolUse hook
- Skill auto-detects and configures the hook on first use
- Hook written in pure bash (no external dependencies)
- Replaced `install-skill` command with an interactive installer (`install.md`) — Claude guides the user through install scope selection and hook setup, with output in the user's system language

## v0.0.1

- Initial release: structured three-layer architecture review
