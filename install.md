# Arch Guardian Installer

You are a Claude Code instance. The user wants to install the Arch Guardian skill. Follow these steps and interact with the user at each stage.

**Language:** Detect the user's preferred language from their system locale (run `echo $LANG` or `locale`) and use that language for all output throughout this installation. If detection fails, default to English.

---

## Step 1: Gather Decisions (single interaction)

Run all environment checks first, then ask ALL questions in **one single `AskUserQuestion` call**.

**Environment checks (silent, before asking anything):**
- Run `pwd` to get the current working directory.
- Run `ls .claude/settings.json .claude/settings.local.json 2>/dev/null` to detect which settings files already exist.

**REQUIRED: Call the `AskUserQuestion` tool exactly once with both questions below.**

**Single `AskUserQuestion` call with two questions at once:**

1. question: `"Where would you like to install Arch Guardian?"`
   - Build options from environment check results. Append `"(already exists)"` to settings files that are present:
     - `"Current project, shared — .claude/settings.json (committed to git, applies to the whole team)"`
     - `"Current project, local only — .claude/settings.local.json (gitignored, applies to your machine only)"`
     - `"Global — ~/.claude/, active across all your projects"`

2. question: `"Configure the change impact speed-check hook? The hook listens on all Bash tool calls but only activates for git commit — running a lightweight architecture check on staged changes, surfacing potential issues in one line, silent otherwise."`
   - options:
     - `"Yes — configure the hook"`
     - `"No — skip for now (can be configured later)"`

After the user submits, derive:
- `BASE_PATH`: `<pwd>/.claude` (current project options) or `~/.claude` expanded to absolute path (global)
- `SETTINGS_FILE`: `<pwd>/.claude/settings.json`, `<pwd>/.claude/settings.local.json`, or `~/.claude/settings.json` — based on Q1; ignored if Q2 = No

---

## Step 2: Install Skill Files

1. Create directory `$BASE_PATH/skills/arch-guardian/scripts/`
2. Fetch SKILL.md and write to `$BASE_PATH/skills/arch-guardian/SKILL.md`:
   ```
   https://raw.githubusercontent.com/larryonline/arch-guardian/main/skills/arch-guardian/SKILL.md
   ```
3. Fetch pre-commit-hook.sh and write to `$BASE_PATH/skills/arch-guardian/scripts/pre-commit-hook.sh`:
   ```
   https://raw.githubusercontent.com/larryonline/arch-guardian/main/skills/arch-guardian/scripts/pre-commit-hook.sh
   ```
4. `chmod +x $BASE_PATH/skills/arch-guardian/scripts/pre-commit-hook.sh`

---

## Step 3: Configure Hook (if selected)

If Question B = Yes: read `SETTINGS_FILE` (treat as `{}` if it doesn't exist), append the following to the `hooks.PreToolUse` array without overwriting existing entries, then write back:

```json
{
  "matcher": "Bash",
  "hooks": [
    {
      "type": "command",
      "command": "$BASE_PATH/skills/arch-guardian/scripts/pre-commit-hook.sh"
    }
  ]
}
```

> Note: replace `$BASE_PATH` with the actual absolute path.

---

## Step 4: Print Install Summary

Output a concise summary:

- Skill files installed to: `$BASE_PATH/skills/arch-guardian/`
- Hook: configured in `SETTINGS_FILE` (only activates for `git commit`, silent on all other Bash calls) — or skipped
- How to use: mention architecture-related topics in Claude Code to trigger a review (e.g. "Review the architecture impact of this change")

**Important:** Restart Claude Code (close and reopen the session) for the skill to be recognized — skills are loaded at session startup.
