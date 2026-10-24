# Tasks: Waitlist landing page

- [ ] 1. Create the Astro project with TypeScript, Tailwind, Vitest, Playwright, and the npm scripts from `AGENTS.md` — done when: `npm run dev`, `npm test`, and `npm run lint` all run
- [ ] 2. Build the home page with name, description, and button (R1) — done when: Playwright test sees the 3 elements
- [ ] 3. Create the `waitlist` table in Supabase (R2, R5) — done when: table exists with `UNIQUE` on email
- [ ] 4. Write `lib/waitlist.ts` with validation and save logic (R2, R4, R5) — done when: Vitest tests pass for valid, invalid, and duplicate emails
- [ ] 5. Build the form with browser validation (R4) — done when: an invalid email shows an error without sending
- [ ] 6. Create the `/api/waitlist` endpoint using `lib/waitlist.ts` (R2, R4, R5) — done when: Vitest tests pass
- [ ] 7. Send the confirmation email with Resend (R3) — done when: the email arrives in my inbox
- [ ] 8. Disable the button while sending (R6) — done when: Playwright test sees the button disabled
- [ ] 9. Add dark mode (R7) — done when: site turns dark with the system in dark mode
- [ ] 10. Deploy to Vercel and validate every requirement — done when: every row below is ✅

## Validation

| Requirement | How it's checked | Status |
|---|---|---|
| R1. Home page with name, description, button | Playwright `home.spec.ts` | ⏳ pending |
| R2. WHEN a valid email is submitted, save it and say thanks | Vitest `waitlist.test.ts` | ⏳ pending |
| R3. WHEN saved, send a confirmation email | Manual: sign up with my own email | ⏳ pending |
| R4. IF the email is empty or invalid, show an error | Vitest + Playwright | ⏳ pending |
| R5. IF already on the list, show "You're already on the list" | Vitest `waitlist.test.ts` | ⏳ pending |
| R6. WHILE sending, disable the button | Playwright `home.spec.ts` | ⏳ pending |
| R7. WHERE dark mode, show dark theme | Manual: switch system to dark mode | ⏳ pending |
