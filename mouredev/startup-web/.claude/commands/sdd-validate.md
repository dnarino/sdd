---
description: SDD step 6 — check every requirement and update the validation table
---

Validate the most recent spec in `specs/`.

1. Read the spec and the validation table in `tasks.md`.
2. Run `npm test` and `npm run test:e2e`.
3. For every requirement, check it against the code and test results, and set ✅ passes / ❌ fails / ⏳ pending.
4. For each ❌, say whether it's a code bug (fix the code) or an unclear requirement (fix the spec first). Don't fix anything yet.
5. For checks marked "Manual", tell me exactly what to try by hand.
6. Finish with a one-line verdict: done, or what's left.
