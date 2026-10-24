# startup-web

Website for the startup: a landing page that explains the product and collects emails for the waitlist.

Built with spec-driven development (SDD). See [`docs/sdd-guide.md`](docs/sdd-guide.md).

## Project structure

```
startup-web/
├── .claude/                 # Claude Code configuration only
│   ├── settings.json        #   shared permissions (committed)
│   └── commands/            #   /sdd-* slash commands, one per SDD step
├── docs/
│   ├── constitution.md      # principles every feature must follow
│   ├── sdd-guide.md         # how the SDD process works
│   └── PROMPTS.md           # same prompts as the commands, for other AI tools
├── specs/
│   └── 001-waitlist-landing/
│       ├── spec.md          # WHAT and WHY
│       ├── plan.md          # HOW
│       └── tasks.md         # STEPS + validation table
├── src/                     # application code (created during implementation)
├── tests/                   # tests (created during implementation)
├── AGENTS.md                # rules for any AI agent
├── CLAUDE.md                # Claude Code entry point (imports AGENTS.md)
├── .env.example             # environment variables needed (no real values)
└── .gitignore
```

## Workflow

| Step | Slash command | Output |
|---|---|---|
| 1. Spec | `/sdd-spec <idea>` | `specs/NNN-feature/spec.md` |
| 2. Clarify | `/sdd-clarify` | Open questions answered |
| 3. Plan | `/sdd-plan <preferences>` | `plan.md` |
| 4. Tasks | `/sdd-tasks` | `tasks.md` |
| 5. Implement | `/sdd-implement` | Code for the next task |
| 6. Validate | `/sdd-validate` | Validation table updated |
| Change | `/sdd-change <change>` | Spec → plan → tasks updated |
| Status | `/sdd-status` | Where the project is |

Each new feature gets its own folder: `specs/002-<feature>/`, `specs/003-<feature>/`…

## Commands

| Command | What it does |
|---|---|
| `npm install` | Install dependencies |
| `npm run dev` | Start the local server |
| `npm test` | Unit tests (Vitest) |
| `npm run test:e2e` | Browser tests (Playwright) |
| `npm run lint` | Lint |
| `npm run build` | Production build |

## Setup

1. Copy `.env.example` to `.env` and fill in the values.
2. `npm install`
3. `npm run dev`
