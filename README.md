# agent-skills

Personal Claude Code skill library. Each file in `skills/` is a reusable slash command for
[Claude Code](https://claude.ai/code).

## Install

Copy any skill file to one of:
- `~/.claude/commands/` — available in all projects globally
- `.claude/commands/` inside a project — available in that project only

The filename (without `.md`) becomes the slash command:
```
skills/sync-skills.md  →  /sync-skills
skills/log-issue.md    →  /log-issue
```

## Skills

| Skill | Description |
|---|---|
| [sync-skills](skills/sync-skills.md) | Sync local Claude Code skill files to this repo (`SheldonFung98/agent-skills`) |
| [log-issue](skills/log-issue.md) | Append a bug/issue entry to a project's `DEVLOG.md` from conversation context |

## Usage

### `/log-issue`
Invoke after discovering and fixing a bug. Claude reads the conversation, extracts the symptom,
root cause, and fix, then appends a numbered `BUG-NNN` entry to the project's `DEVLOG.md`.
Requires a `DEVLOG.md` file in the project root with a `## Bug & Issue Registry` section.

### `/sync-skills`
Syncs skill files from `~/.claude/commands/` (and optionally `.claude/commands/`) to this
repo. Clones the repo if not already present at `~/agent-skills/`, copies non-project-specific
skills, commits, and pushes.
