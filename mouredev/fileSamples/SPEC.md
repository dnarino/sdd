# Spec-Driven Development (SDD) Guide

A guide to follow every time I start a new software project (for example, a website for my startup).

**Idea:** before writing code, write down *what* to build, *how* to build it, and *in what order*. Then build it (myself or with an AI agent) following those documents.

---

## 1. The three documents

| File | Answers the question | Analogy (building a house) | Who reads it |
|---|---|---|---|
| `spec.md` | **WHAT** do we build, and **why**? | "I want 3 bedrooms, a big kitchen, and lots of light." | Anyone: me, a client, a teammate |
| `plan.md` | **HOW** do we build it? | The architect's blueprint: materials, structure, pipes. | Developers / AI agent |
| `tasks.md` | **In what order**, step by step? | The builder's checklist: foundation → walls → roof. | Developers / AI agent |

> **Spec = goal · Plan = strategy · Tasks = actions**

Why keep them separate:

- I can change **what** I want without touching tech decisions.
- I can change the **technology** (e.g. Astro → Next.js) and the spec stays the same; only the plan and tasks change.
- An AI agent reads the spec (goal), the plan (rules), and then works through the tasks one by one.

> Other tools use different names for the same idea: Kiro uses `requirements.md → design.md → tasks.md`; GitHub spec-kit uses `spec.md → plan.md → tasks.md`. The [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) plugin automates this flow for AI agents (`/spec → /plan → /build → /test → /review → /ship`); the **Open questions** and **Boundaries** ideas in this guide come from it.

---

## 2. Workflow

1. **Write `spec.md`.** Describe the problem, the users, and the requirements in EARS. No technology.
2. **Clarify the spec.** Write down every doubt in **Open questions** and answer them all. Is every requirement testable? Did I write the `IF … THEN` (error) cases?
3. **Write `plan.md`.** Choose the stack, structure, data model, and key decisions.
4. **Write `tasks.md`.** Break the plan into small tasks that can each be checked off.
5. **Implement task by task.** Write the code for one task, run its "done" check, then mark it `- [x]`.
6. **Validate.** Check every spec requirement with a test or by hand, and record the result in the validation table.
7. **When something changes:** update the spec first, then the plan, then the tasks. Never only the code.

> `spec.md` (what) → review → `plan.md` (how) → `tasks.md` (steps) → **implementation** (code) → **validation** (does it do what the spec says?)

Ready-to-copy prompts for each step are in [`PROMPTS.md`](PROMPTS.md).

---

## 3. `spec.md`: WHAT and WHY

What goes inside:

- **Goal:** the problem and why it matters (1–3 sentences).
- **Users:** who will use it.
- **User stories:** `As a <user>, I want <action>, so that <benefit>.`
- **Requirements:** written with EARS (see below).
- **Out of scope:** what this version will *not* do.
- **Open questions:** doubts that must be answered before writing the plan.

Rule: **no technology** in the spec (no React, no database names, no hosting).

### Open questions (clarification phase)

Before writing the plan, list everything that is unclear or not decided yet. Read each requirement and ask: *What happens if…? Who…? How many…? Is this needed now?*

- Write each doubt as a question: `- [ ] Q1. <question>`.
- Answer each one (myself, with the client, or with the AI), then mark it `- [x]` and write the answer.
- Every answer ends up in the spec: it becomes a **new or clearer requirement**, or goes into **Out of scope**.
- **Don't start the plan while there are unanswered questions.**

### EARS: how to write requirements

**EARS** = *Easy Approach to Requirements Syntax*. Fixed sentence templates that make requirements clear and testable.

| Pattern | Keyword | Template | Example |
|---|---|---|---|
| **Ubiquitous** (always true) | — | The `<system>` shall `<response>` | The API shall return JSON. |
| **Event-driven** | `WHEN` | WHEN `<trigger>`, the `<system>` shall `<response>` | WHEN the user clicks "Save", the system shall save the draft. |
| **State-driven** | `WHILE` | WHILE `<state>`, the `<system>` shall `<response>` | WHILE offline, the app shall queue requests locally. |
| **Unwanted behavior** | `IF … THEN` | IF `<bad condition>`, THEN the `<system>` shall `<response>` | IF the password is wrong 3 times, THEN the system shall lock the account for 15 min. |
| **Optional feature** | `WHERE` | WHERE `<feature is present>`, the `<system>` shall `<response>` | WHERE 2FA is enabled, the system shall ask for a code at login. |
| **Complex** | combined | WHILE … WHEN … the `<system>` shall … | WHILE logged in, WHEN the session hits 30 min of inactivity, the system shall log the user out. |

#### Which pattern to use?

You don't use only one. Pick the right pattern for each requirement. Ask yourself what makes the requirement apply:

| If the behavior… | Use |
|---|---|
| is always true, with no condition | **Ubiquitous** |
| happens in response to something | **WHEN** (event) |
| applies only while the system is in a certain state | **WHILE** (state) |
| handles an error, a failure, or bad input | **IF … THEN** (unwanted) |
| depends on a feature, setting, or variant being present | **WHERE** (optional) |
| needs more than one of the above | **Complex** (combine them) |

#### Example: one feature, several patterns

**User story:** As a user, I want to log in so that I can reach my account.

1. The system shall store passwords hashed with bcrypt. *(ubiquitous)*
2. WHEN the user submits valid credentials, the system shall start a session and redirect to the dashboard. *(event)*
3. IF the credentials are invalid, THEN the system shall show "Invalid email or password". *(unwanted)*
4. IF the user fails login 5 times in 10 minutes, THEN the system shall lock the account for 15 minutes. *(unwanted)*
5. WHERE 2FA is enabled, WHEN the password is valid, the system shall ask for a one-time code. *(complex: optional + event)*
6. WHILE the session is active, the system shall refresh the token every 15 minutes. *(state)*

#### Tips

- **One behavior per requirement.** If a sentence needs "and" to describe two responses, split it into two.
- **Don't force a keyword.** If the behavior is always true, write it as ubiquitous rather than inventing a WHEN.
- **Write the IF … THEN cases for every feature.** In practice they're what gets skipped most, and they're where bugs hide.
- **Keep Complex requirements short.** Two conditions is usually the limit; beyond that, split the requirement.
- **Most acceptance criteria will be WHEN and IF … THEN.** Ubiquitous, WHILE, and WHERE come up less often.

#### Spanish version of the patterns

| Patrón | Tipo | Plantilla | Ejemplo |
|---|---|---|---|
| **Ubicuo** | Siempre activo | `EL SISTEMA [comportamiento]` | EL SISTEMA almacenará todos los datos en un único archivo JSON local legible. |
| **Evento** | Dirigido por evento | `CUANDO [evento], EL SISTEMA [respuesta]` | CUANDO el usuario ejecute `habits done estudiar`, EL SISTEMA registrará la fecha de hoy. |
| **Estado** | Dirigido por estado | `MIENTRAS [estado], EL SISTEMA [respuesta]` | MIENTRAS no exista ningún hábito, EL SISTEMA invitará a crear el primero. |
| **NoDeseado** | Comportamiento no deseado | `SI [condición], ENTONCES EL SISTEMA [respuesta]` | SI el hábito ya está completado hoy, ENTONCES EL SISTEMA lo comunicará sin duplicarlo. |
| **Opcional** | Funcionalidad opcional | `DONDE [opción activa], EL SISTEMA [respuesta]` | DONDE se pase la opción `--verbose`, EL SISTEMA mostrará info de depuración. |

---

## 4. `plan.md`: HOW

What goes inside:

- **Tech stack:** language, framework, libraries, hosting.
- **Structure:** folders and main files.
- **Data model:** what data is stored and in what shape.
- **Components:** the main pieces and how they connect.
- **Decisions and trade-offs:** why I chose X over Y, and the risks.
- **Boundaries:** what the AI agent (or any developer) must always do, must ask before doing, and must never do.

Rule: every decision in the plan should serve a requirement in the spec.

### Boundaries: Always / Ask first / Never

Clear limits so an AI agent doesn't make big decisions on its own or break important things.

| Level | Meaning | Examples |
|---|---|---|
| ✅ **Always** | Do it without asking | Run the tests before marking a task done. Follow the folder structure. |
| ⚠️ **Ask first** | Stop and ask me before doing it | Add a new library. Change the data model. Change something in the spec. |
| 🚫 **Never** | Not allowed, ever | Commit API keys or passwords. Delete data. Build features that aren't in the spec. |

Tip: copy the boundaries into the **Reglas** section of `AGENTS.md`, so the AI agent reads them on every task.

---

## 5. `tasks.md`: IN WHAT ORDER

What goes inside:

- A numbered checklist (`- [ ]`), in the order to build.
- Small tasks: each one should be done and testable in one sitting.
- For each task, which spec requirement(s) it covers.
- A "done" check: a test, or how to verify it by hand.

---

## 6. Implementation: DO IT

Implementation doesn't need its own document. It happens in **`tasks.md` + the code**.

How to work:

1. Take the **next unchecked task** in `tasks.md`. Only one at a time.
2. Write the code for that task (myself or with an AI agent).
3. Run its **"done" check** (the test or the manual check written in the task).
4. If it passes, mark it done: `- [ ]` → `- [x]`.
5. If something in the spec or plan turns out to be wrong or missing, **stop and update those documents first**, then continue.

Rules:

- Don't add features that aren't in the spec. If I want something new, it goes into the spec first.
- Respect the **Boundaries** in the plan: anything under "Ask first" means stop and ask.
- `tasks.md` is my progress record: at any moment it shows what's done and what's left.
- `AGENTS.md` tells the AI agent which commands to run when it finishes a task (tests, lint).

---

## 7. Validation: DOES IT DO WHAT THE SPEC SAYS?

Validation checks the finished software against the **spec**, not against the code. It doesn't need its own document either. It lives in:

| Where | What goes there |
|---|---|
| **Tests in the code** | Automated checks. Ideally each EARS requirement becomes at least one test. |
| **`tasks.md`** | A **Validation** table at the end: one row per requirement, how it's checked, and its status. |
| **`AGENTS.md`** | The commands to run when a task is finished (section "Al terminar cualquier tarea"). |

EARS requirements turn into tests almost directly:

| EARS | Test |
|---|---|
| WHEN `<trigger>` … | **Given** the starting situation, **when** the trigger happens… |
| … THE SYSTEM shall `<response>` | **then** check that the response happened. |
| IF `<bad condition>`, THEN … | A test that forces the bad condition and checks the error handling. |

Statuses for the validation table:

- ✅ **passes**: the requirement works.
- ❌ **fails**: either the code has a bug (fix the code) or the requirement was unclear (fix the spec first, then the code).
- ⏳ **pending**: not built or not checked yet.

The project is **finished** only when every requirement is ✅.

---

## 8. Full example: website for my startup

A landing page that explains the product and collects emails for a waitlist.

### `spec.md`

> **Goal:** Explain what the startup does and collect emails from interested people before launch.
>
> **Users:** potential customers and investors visiting the site.
>
> **User story:** As a visitor, I want to understand the product and join the waitlist, so that I'm notified when it launches.
>
> **Requirements:**
> - R1. THE SYSTEM shall show a home page with the product name, a short description, and a "Join the waitlist" button.
> - R2. WHEN the visitor submits the waitlist form with a valid email, THE SYSTEM shall save the email and show a thank-you message.
> - R3. WHEN a new email is saved, THE SYSTEM shall send a confirmation email to the visitor.
> - R4. IF the email field is empty or invalid, THEN THE SYSTEM shall show an error and not save it.
> - R5. IF the email is already on the waitlist, THEN THE SYSTEM shall show "You're already on the list" without saving it again.
> - R6. WHILE the form is sending, THE SYSTEM shall disable the "Join" button.
> - R7. WHERE the visitor's device uses dark mode, THE SYSTEM shall show the dark theme.
>
> **Out of scope:** user accounts, payments, admin panel, multiple languages.
>
> **Open questions:**
> - [x] Q1. What if someone signs up twice with the same email? → Show "You're already on the list" and don't save it again. *(became R5)*
> - [x] Q2. Do we send an email after signing up? → Yes, a confirmation email. *(became R3)*
> - [x] Q3. Spanish and English? → English only for now. *(added to Out of scope)*
> - [x] Q4. Do I need an admin page to see the emails? → No, I'll look at them in the database. *(added to Out of scope)*

### `plan.md`

> **Tech stack:**
> - Framework: Astro (fast static site)
> - Styles: Tailwind CSS
> - Waitlist storage: Supabase (a single `waitlist` table)
> - Emails: Resend API
> - Hosting: Vercel
> - Tests: Vitest (API logic) + Playwright (browser)
>
> **Structure:**
> ```
> src/
>   pages/index.astro       → home page + waitlist form
>   pages/api/waitlist.ts   → receives the form, saves, sends email
>   components/Form.astro   → form with validation
> tests/
>   waitlist.test.ts        → API tests (Vitest)
>   home.spec.ts            → browser tests (Playwright)
> ```
>
> **Data model:** `waitlist(id, email UNIQUE, created_at)`
>
> **Decisions:**
> - Email is validated in the browser and again on the server.
> - `UNIQUE` on email prevents duplicates (R5).
> - Dark mode uses the CSS `prefers-color-scheme` (R7).
>
> **Boundaries:**
> - ✅ Always: run `npm test` before marking a task done; keep the API keys in `.env`.
> - ⚠️ Ask first: adding a new library; changing the `waitlist` table; changing any text the visitor sees.
> - 🚫 Never: commit `.env` or API keys; delete rows from the `waitlist` table; add login, payments, or other out-of-scope features.

### `tasks.md` (during implementation)

Tasks 1–6 are done, 7–9 are still pending. Each task has its "done" check.

> - [x] 1. Create the Astro project with Tailwind — *done: `npm run dev` opens the site*
> - [x] 2. Build the home page with name, description, and button *(R1)* — *done: Playwright test sees the 3 elements*
> - [x] 3. Create the `waitlist` table in Supabase *(R2, R5)* — *done: table exists with `UNIQUE` on email*
> - [x] 4. Build the form with browser validation *(R4)* — *done: an invalid email shows an error*
> - [x] 5. Create the `/api/waitlist` endpoint that saves the email *(R2, R4, R5)* — *done: Vitest tests pass*
> - [x] 6. Send the confirmation email with Resend *(R3)* — *done: email arrives in my inbox*
> - [ ] 7. Disable the button while sending *(R6)* — *done when: Playwright test sees the button disabled*
> - [ ] 8. Add dark mode *(R7)* — *done when: site turns dark with the system in dark mode*
> - [ ] 9. Deploy to Vercel and validate every requirement — *done when: every row below is ✅*
>
> **Validation**
>
> | Requirement | How it's checked | Status |
> |---|---|---|
> | R1. Home page with name, description, button | Playwright `home.spec.ts` | ✅ |
> | R2. WHEN a valid email is submitted, save it and say thanks | Vitest `waitlist.test.ts` | ✅ |
> | R3. WHEN saved, send a confirmation email | Manual: sign up with my own email | ✅ |
> | R4. IF the email is empty or invalid, show an error | Vitest + Playwright | ✅ |
> | R5. IF already on the list, show "You're already on the list" | Vitest `waitlist.test.ts` | ❌ fails |
> | R6. WHILE sending, disable the button | Playwright `home.spec.ts` | ⏳ pending (task 7) |
> | R7. WHERE dark mode, show dark theme | Manual: switch system to dark mode | ⏳ pending (task 8) |

**What to do with R5 ❌:** the test shows that a duplicate email returns a generic "Something went wrong" error. The spec is clear ("You're already on the list"), so it's a code bug: fix `/api/waitlist.ts` to catch the duplicate error and return the right message, rerun the test, then mark R5 ✅. If the spec had been unclear, I'd fix the spec first.

### How they connect

| Spec (WHAT) | → Plan (HOW) | → Task (DO) | → Validation (CHECK) |
|---|---|---|---|
| R2. WHEN a valid email is submitted, save it | `/api/waitlist.ts` + Supabase | Tasks 3, 5 | Vitest `waitlist.test.ts` |
| R4. IF the email is invalid, show an error | Validate in browser and on server | Tasks 4, 5 | Vitest + Playwright |
| R5. IF already on the list, don't duplicate | `UNIQUE` constraint on `email` | Tasks 3, 5 | Vitest `waitlist.test.ts` |
| R7. WHERE dark mode is on, show dark theme | CSS `prefers-color-scheme` | Task 8 | Manual check |

If next month I switch from **Astro** to **Next.js**: the spec **doesn't change**, the plan **changes**, and the tasks are **regenerated**.

---

## 9. Templates to copy

### `spec.md`

```markdown
# Spec: <project name>

## Goal
<What problem does it solve, and why does it matter? 1–3 sentences.>

## Users
- <who uses it>

## User stories
- As a <user>, I want <action>, so that <benefit>.

## Requirements
- R1. THE SYSTEM shall <behavior>.
- R2. WHEN <event>, THE SYSTEM shall <response>.
- R3. IF <bad condition>, THEN THE SYSTEM shall <response>.
- R4. WHILE <state>, THE SYSTEM shall <response>.
- R5. WHERE <option is active>, THE SYSTEM shall <response>.

## Out of scope
- <what this version will NOT do>

## Open questions
- [ ] Q1. <question> → <answer> (became R? / added to Out of scope)
```

### `plan.md`

```markdown
# Plan: <project name>

## Tech stack
- Language / framework:
- Libraries:
- Data storage:
- Hosting:
- Tests:

## Structure
<folders and main files, including tests>

## Data model
<tables / JSON shape>

## Decisions
- <decision> — because <reason> (covers R?)

## Boundaries
- ✅ Always: <do without asking>
- ⚠️ Ask first: <stop and ask before doing>
- 🚫 Never: <not allowed>
```

### `tasks.md`

```markdown
# Tasks: <project name>

- [ ] 1. <small task> (R?) — done when: <test or manual check>
- [ ] 2. <small task> (R?) — done when: <test or manual check>
- [ ] 3. Validate every requirement — done when: every row below is ✅

## Validation

| Requirement | How it's checked | Status |
|---|---|---|
| R1. <short requirement> | <test file or manual check> | ⏳ pending |
| R2. <short requirement> | <test file or manual check> | ⏳ pending |
```

---

## 10. Checklists

### Before building

- [ ] The spec has no technology in it.
- [ ] Every requirement uses an EARS pattern and describes one behavior.
- [ ] Every feature has its `IF … THEN` error cases.
- [ ] "Out of scope" is written down.
- [ ] Every open question is answered, and each answer is in the spec.
- [ ] Every plan decision points to a requirement.
- [ ] Boundaries (Always / Ask first / Never) are written in the plan and copied to `AGENTS.md`.
- [ ] Every task is small and points to the requirement(s) it covers.
- [ ] Every requirement is covered by at least one task.
- [ ] Every task has a "done" check.

### Before calling it finished

- [ ] Every task in `tasks.md` is marked `- [x]`.
- [ ] Every requirement has a row in the validation table, and every row is ✅.
- [ ] All tests pass (the commands in `AGENTS.md`).
- [ ] Nothing was built that isn't in the spec.
- [ ] If anything changed while building, the spec and plan were updated to match.

