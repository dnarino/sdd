# Spec: Waitlist landing page

## Goal
Explain what the startup does and collect emails from interested people before launch.

## Users
- Potential customers visiting the site.
- Investors checking out the startup.

## User stories
- As a visitor, I want to understand the product and join the waitlist, so that I'm notified when it launches.

## Requirements
- R1. THE SYSTEM shall show a home page with the product name, a short description, and a "Join the waitlist" button.
- R2. WHEN the visitor submits the waitlist form with a valid email, THE SYSTEM shall save the email and show a thank-you message.
- R3. WHEN a new email is saved, THE SYSTEM shall send a confirmation email to the visitor.
- R4. IF the email field is empty or invalid, THEN THE SYSTEM shall show an error and not save it.
- R5. IF the email is already on the waitlist, THEN THE SYSTEM shall show "You're already on the list" without saving it again.
- R6. WHILE the form is sending, THE SYSTEM shall disable the "Join" button.
- R7. WHERE the visitor's device uses dark mode, THE SYSTEM shall show the dark theme.

## Out of scope
- User accounts, payments, admin panel.
- Multiple languages (English only for now).

## Open questions
- [x] Q1. What if someone signs up twice with the same email? → Show "You're already on the list" and don't save it again. *(became R5)*
- [x] Q2. Do we send an email after signing up? → Yes, a confirmation email. *(became R3)*
- [x] Q3. Spanish and English? → English only for now. *(added to Out of scope)*
- [x] Q4. Do I need an admin page to see the emails? → No, I'll look at them in the database. *(added to Out of scope)*
- [ ] Q5. What are the product name and the short description? → <fill in>
