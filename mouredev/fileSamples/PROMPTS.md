# Prompts Library

**What this file is:** a **reusable library** of prompts to send to the AI agent at each step of the SDD flow described in `SPEC.md`.

**What this file is NOT:** a per-project log. Don't write project history here. Each project's real information lives in its own `spec.md`, `plan.md`, and `tasks.md`; rules for the agent live in `AGENTS.md`.

**How to use it:**

1. Copy the prompt for the step you're on.
2. Replace the `<...>` parts.
3. Paste it into the AI agent.
4. If a prompt gives bad results, **improve it here**, so every future project benefits.

> Flow: 1. Spec → 2. Clarify → 3. Plan → 4. Tasks → 5. Implement → 6. Validate

---

## 1. Create the spec

```
Read my idea below and write spec.md following SPEC.md (section 3 and the template in section 9).
Use EARS for the requirements (R1, R2…) and include the IF … THEN error cases.
No technology in the spec. Add any doubts to "Open questions".
Don't write the plan yet.

Idea: <describe my project>
```

## 2. Clarify

```
Read spec.md and ask me questions, one by one, about anything unclear or missing.
Add each question to "Open questions". After I answer, update the spec:
turn each answer into a new or clearer requirement, or add it to "Out of scope".
Don't write the plan until every question is answered.
```

## 3. Create the plan

```
Read spec.md and write plan.md following SPEC.md (section 4 and the template in section 9):
tech stack, structure, data model, decisions, and boundaries (Always / Ask first / Never).
Every decision must point to a requirement (R?).
Then copy the boundaries into the "Reglas" section of AGENTS.md.

Preferences: <e.g. Astro + Tailwind, free hosting>
```

## 4. Create the tasks

```
Read spec.md and plan.md and write tasks.md following SPEC.md (section 5 and the template in section 9).
Small tasks in build order, each with the requirement(s) it covers (R?) and a "done when" check.
Add the validation table with one row per requirement, all ⏳ pending.
Make sure every requirement is covered by at least one task.
```

## 5. Implement

```
Read AGENTS.md, spec.md, plan.md, and tasks.md.
Implement ONLY the next unchecked task. Run its "done" check and the commands in AGENTS.md.
If it passes, mark it [x]. Then stop and tell me what you did.
If anything in the spec or plan is wrong or missing, or something is under "Ask first", stop and ask me.
```

## 6. Validate

```
Check every requirement in spec.md against the code and tests.
Update the validation table in tasks.md with ✅ / ❌ / ⏳.
For each ❌, explain whether it's a code bug (fix the code) or an unclear requirement (fix the spec first).
```

---

## Extra prompts

### Change something mid-project

```
I want to change: <the change>.
Update spec.md first, then plan.md, then tasks.md. Show me what changed in each file before touching any code.
```

### Resume work in a new session

```
Read AGENTS.md, spec.md, plan.md, and tasks.md.
Tell me in 3 lines where the project is: what's done, what's next, and any ❌ in the validation table.
```
