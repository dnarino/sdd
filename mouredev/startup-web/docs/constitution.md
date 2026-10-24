# Constitution

The non-negotiable principles for every feature in this project. Specs and plans must follow them; if one has to be broken, it's written down as a decision in `plan.md` with the reason.

## 1. Spec first
- No code without an approved spec in `specs/<NNN-feature>/`.
- Requirements use EARS and include the `IF … THEN` error cases.
- Changes go spec → plan → tasks → code, never only code.

## 2. Tested
- Every requirement has at least one check in the validation table.
- Logic is covered by unit tests (Vitest); user flows by end-to-end tests (Playwright).
- A task is done only when its "done" check and `npm test` pass.

## 3. Secure
- Secrets live only in environment variables (`.env`, never committed).
- All user input is validated on the server, even if the browser already validates it.
- Dependencies are added only when needed and asked first.

## 4. Accessible
- Semantic HTML, labels on every form field, keyboard navigation, visible focus.
- Text contrast meets WCAG 2.2 AA.

## 5. Fast
- Static by default; JavaScript only where interaction needs it.
- Target Lighthouse score ≥ 90 in Performance, Accessibility, Best Practices, and SEO.

## 6. Simple
- Prefer the simplest solution that meets the spec.
- No features, abstractions, or options that the spec doesn't ask for.
