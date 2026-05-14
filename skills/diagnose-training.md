---
description: Diagnose a training log and append findings to docs/training_diagnosis_v1.md
allowed-tools: Read, Edit, Bash, Write
---

A new training log is available. Your job is to analyse it and append a new session block to the training diagnosis document.

## Steps

1. **Identify the log file** from the conversation context. If not specified, look for the most recent `training_log_*.txt` in the project root:
   ```bash
   ls -t training_log*.txt 2>/dev/null | head -5
   ```

2. **Read the log** thoroughly. Extract:
   - Epoch range covered
   - Per-epoch mAP@50:95 values (from `coco_eval_bbox` in eval output)
   - Per-epoch 3D metrics if present: Ground Center error (px), GP Corners error (px), CP Corners error (px)
   - LR values and any plateau-reducer events (`PlateauLRReducer` log lines)
   - 3D loss warmup state (look for warmup factor lines)
   - Any loss spikes: steps where overall/bbox loss is >10× the running median for that epoch
   - Any Python warnings, errors, CUDA errors, or NaN/Inf in losses
   - `best_stat` lines at epoch end to track best-so-far

3. **Read the current diagnosis doc** to find the next session number and the last epoch covered:
   ```bash
   find . -name "training_diagnosis_v1.md" -maxdepth 4 | head -1
   ```

4. **Diagnose**:
   - Is training progressing (new bests, loss decreasing)?
   - Is the plateau reducer behaving correctly (`_wait` counter, triggered reductions)?
   - Are there data pipeline anomalies (loss spikes, NaN/Inf)?
   - Are there any new warnings or errors not seen before?
   - Are any previously flagged issues (ISSUE-S*) now resolved or worsening?
   - What is the projected trajectory (mAP trend, estimated epochs to next milestone)?

5. **Append a new session block** to the diagnosis doc using this template:

```markdown
---

## Session N — Epochs X–Y (<brief descriptor>)

**Log file**: `training_log_N.txt`
**Date**: YYYY-MM-DD
**Command**: (resume or fresh start command from log header or context)

### Metrics (epochs X–Y)

| Epoch | mAP@50:95 | GC Error (px) | GP Corners (px) | CP Corners (px) | Notes |
|-------|-----------|---------------|-----------------|-----------------|-------|
| X     | ...       | ...           | ...             | ...             | ...   |

### Issues Found

#### ISSUE-SN-NN — <Title> (if any new issues)

**Symptom**: ...
**Root cause**: ...
**Fix / Fix needed**: ...
**Severity**: Low / Medium / High

### Actions Taken

- ...

**Resume command** (if applicable):
```bash
...
```
```

6. Replace the `## Session N — (pending)` placeholder line at the end of the doc with the new block, then append a fresh `## Session N+1 — (pending)` placeholder.

7. **Report** a concise summary: epoch range, final mAP, key issues found, and whether any config change is recommended before the next resume.
