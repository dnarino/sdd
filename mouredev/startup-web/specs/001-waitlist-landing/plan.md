# Plan: Waitlist landing page

## Tech stack
- Language / framework: TypeScript + Astro (static site, server endpoint for the form)
- Styles: Tailwind CSS
- Data storage: Supabase (Postgres)
- Emails: Resend API
- Hosting: Vercel
- Tests: Vitest (API logic) + Playwright (browser)

## Structure
```
src/
  pages/index.astro            → home page + waitlist form (R1, R7)
  pages/api/waitlist.ts        → receives the form, saves, sends email (R2–R5)
  components/WaitlistForm.astro → form with browser validation (R4, R6)
  lib/waitlist.ts              → validation + save logic, easy to unit test
tests/
  waitlist.test.ts             → API/logic tests (Vitest)
  home.spec.ts                 → browser tests (Playwright)
```

## Data model
`waitlist(id uuid PK, email text UNIQUE NOT NULL, created_at timestamptz DEFAULT now())`

## Decisions
- Email is validated in the browser and again on the server — the constitution requires server validation (R4).
- `UNIQUE` on email prevents duplicates; the endpoint catches the duplicate error and returns "You're already on the list" (R5).
- The confirmation email is sent only after the save succeeds (R3).
- Dark mode uses the CSS `prefers-color-scheme`, no JavaScript needed (R7).
- Logic lives in `lib/waitlist.ts` so it can be tested without a browser.

## Boundaries
- ✅ Always: run `npm test` before marking a task done; keep API keys in `.env`.
- ⚠️ Ask first: adding a new library; changing the `waitlist` table; changing any text the visitor sees.
- 🚫 Never: commit `.env` or API keys; delete rows from the `waitlist` table; add login, payments, or other out-of-scope features.
