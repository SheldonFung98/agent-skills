---
description: Sync Claude Code skill files to the personal agent-skills GitHub repo (SheldonFung98/agent-skills)
allowed-tools: Bash, Read, Write
---

Sync local Claude Code skill files to the personal agent-skills repository at
`git@github.com:SheldonFung98/agent-skills.git`.

## Steps

1. **Locate skill files** to sync:
   - Global skills: `~/.claude/commands/*.md`
   - Project skills (if in a project): `.claude/commands/*.md` (exclude files that are
     clearly project-specific and not reusable)

2. **Clone or pull** the agent-skills repo:
   ```bash
   if [ -d ~/agent-skills/.git ]; then
     git -C ~/agent-skills pull --rebase
   else
     git clone git@github.com:SheldonFung98/agent-skills.git ~/agent-skills
   fi
   ```

3. **Copy** skill files into `~/agent-skills/skills/`:
   ```bash
   mkdir -p ~/agent-skills/skills
   # copy files...
   ```

4. **Ensure a README.md exists** at `~/agent-skills/README.md`. If it does not exist,
   create one with:
   - A one-paragraph description of the repo (personal Claude Code skill library)
   - A table listing each skill file, its description (from frontmatter), and usage
   - Instructions: "Install: copy any skill file to `~/.claude/commands/` or `.claude/commands/`"

5. **Commit and push**:
   ```bash
   git -C ~/agent-skills add -A
   git -C ~/agent-skills commit -m "sync skills $(date +%Y-%m-%d)"
   git -C ~/agent-skills push
   ```

6. Report which files were synced and the commit hash.

## Rules
- Never sync files that contain secrets, API keys, or project-specific paths that would
  break on another machine.
- Skills that reference a hardcoded `DEVLOG.md` path or project-local file are project-specific
  — skip them unless the user explicitly asks to include them.
- If `git push` fails (e.g., SSH key not available), report the error clearly and suggest
  `ssh-add ~/.ssh/id_rsa` as a first step.
