---
name: checkpoint
description: Record ShopLite progress — tick finished checklist items in the Notion tracker, move the "Now" line, add a progress-log entry, and tag git when a phase is complete. Use when the user says a task or phase is done.
disable-model-invocation: true
argument-hint: "[what was finished, optional]"
---

# /checkpoint

Writes to Notion and git, so **confirm before writing**.

1. Fetch the Notion tracker page `3f1251d7d623802e8fd8d726bc974d76`.
2. Work out which unchecked items are now done:
   - from `$ARGUMENTS` if given,
   - otherwise from the work in this session and the repo (`git log`, files present, tests passing).
   Only count an item done if there is evidence: the code exists and `./mvnw verify` (or the relevant module build) passes. Run the build if it was not run since the last change.
3. Show the user the plan and wait for a yes:
   ```
   Tick:
   - [x] item …
   Now → Phase N – Name
   Log: YYYY-MM-DD – …
   Git tag: phase-N-done (only if every item in the phase is ticked)
   ```
4. On yes, update the page with the Notion update tool using `update_content`:
   - replace each `- [ ] <exact item text>` with `- [x] <exact item text>`,
   - replace the `**Now:** …` line,
   - append one line to the Progress log: `- YYYY-MM-DD – <what was done>`.
   Fetch again and confirm the changes landed.
5. If the phase is complete: make sure the work is committed, then `git tag -a phase-N-done -m "Phase N – Name complete"`. Ask before `git push` and `git push --tags`.
6. Reply with what changed in two or three lines.
