---
name: to-answer
description: Turn something built in ShopLite into an interview answer card grounded in the real code, and (after confirmation) add it to the user's Master Interview Reviewer in Notion. Use when a feature or phase is finished, or the user asks "how would I explain this in an interview?".
argument-hint: "<topic, e.g. outbox | stock race condition | jwt validation>"
---

# /to-answer $ARGUMENTS

## 1. Build the card from the code

Read the code for `$ARGUMENTS` in this repo, then write the card in this shape:

```
### <Question as an interviewer would ask it>
**Short answer (say this first):** 2–3 sentences, plain words.
**In ShopLite:** what I built and where — `path/File.java` — and the problem it solved.
**How it works:** 3–5 bullets.
**Follow-ups they ask:**
- <question> → <short answer>
- <question> → <short answer>
**Trap to avoid:** one common wrong statement.
```

Rules: first person ("I built…"), only claims the code supports, no buzzword padding. If a STAR story fits (a bug found, a race reproduced), add a 4-line STAR version.

## 2. Show it and ask

Show the card in chat and ask: "Add this to your Master Interview Reviewer?"

## 3. On yes, write to Notion

- Page: Master Interview Reviewer `3ea251d7d62381a987cbc0ec5161ab77`. Fetch it first.
- If there is no `## From ShopLite` toggle heading yet, append one at the end of the page: `## From ShopLite {toggle="true"}`.
- Append the card inside that toggle (children indented with a tab). Never edit or reorder the user's existing sections.
- Fetch again to confirm, then reply in one line.
