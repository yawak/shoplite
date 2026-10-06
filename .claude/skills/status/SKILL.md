---
name: status
description: Show where the ShopLite build stands — current phase, progress per phase, and the next tasks — by reading the Notion tracker and the git repo. Use at the start of a session or when asked "where are we?".
---

# /status

Read-only. Never change Notion or git here.

1. Fetch the Notion tracker page `3f1251d7d623802e8fd8d726bc974d76` with the Notion connector's fetch tool.
2. From the page, extract:
   - the **Now** line in the callout at the top,
   - for each phase toggle under "Build checklist": checked `- [x]` vs total `- [ ]` + `- [x]` items,
   - the first 3–5 unchecked items of the current phase,
   - the last 3 lines of the Progress log.
3. From the repo: current branch, `git status --short` (count only), last 3 commits (`git log --oneline -3`), and tags matching `phase-*`.
4. Reply in this shape, nothing more:

```
📍 Now: Phase N – Name   (x/y done)

Phase 0 ✅  Phase 1 4/20  Phase 2 0/15 …

Next up:
- [ ] …
- [ ] …

Repo: branch main · 2 uncommitted files · last commit "…" · tags: phase-0-done
```

5. If the Notion state and the repo disagree (e.g. a box is ticked but the code is clearly missing, or a phase tag exists but boxes are unticked), say so in one line.
