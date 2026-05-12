---
description: Append a bug/issue entry to DEVLOG.md based on the current conversation context
allowed-tools: Read, Edit, Bash
---

A bug or issue has just been found and/or fixed. Your job is to record it in `DEVLOG.md`.

## Steps

1. **Read** `DEVLOG.md` to find the highest existing BUG number (e.g., BUG-004 → next is BUG-005).

2. **Extract** from the conversation context:
   - **Date**: today's date (YYYY-MM-DD)
   - **Symptom**: what the user observed that was wrong (error message, visual artifact, wrong output)
   - **Root cause**: the underlying technical reason (be precise — include data types, function names, library behavior)
   - **Fix**: exactly what changed (code snippet if useful, file + line)
   - **Files**: which files were modified

3. **Append** a new entry to the `## Bug & Issue Registry` section of `DEVLOG.md` using this format:

```
---

### BUG-NNN — <Short Title> (`<primary_file.py>`)

**Date**: YYYY-MM-DD
**Symptom**: <what was observed>
**Root cause**: <technical explanation>
**Fix**: <what changed>
**Files**: `path/to/file.py`
```

4. Confirm to the user: "Logged as BUG-NNN in DEVLOG.md."

## Rules
- Keep entries factual and precise — no fluff, no hedging.
- Root cause must explain *why* the bug existed, not just what it did.
- If the fix involved a code change, include a before/after snippet.
- If the issue was a design decision (not a code defect), use the same format but prefix the title with "DECISION-NNN" instead of "BUG-NNN".
