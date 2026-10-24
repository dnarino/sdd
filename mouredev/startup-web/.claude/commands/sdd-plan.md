---
description: SDD step 3 — write the technical plan for the active spec
argument-hint: [tech preferences]
---

Write `plan.md` for the most recent spec in `specs/`. My preferences: $ARGUMENTS

1. Read `docs/constitution.md`, `docs/sdd-guide.md` (section 4 and the `plan.md` template in section 9), `AGENTS.md`, and the spec.
2. If the spec has unanswered open questions, stop and tell me to run `/sdd-clarify` first.
3. Write tech stack, structure, data model, decisions, and boundaries (Always / Ask first / Never).
4. Every decision must point to a requirement (R?). Nothing may break the constitution without a written reason.
5. If the boundaries differ from those in `AGENTS.md`, show me the difference and ask before changing `AGENTS.md`.
6. Stop after writing the plan. Don't write tasks or code.
