# SDD — Spec-Driven Development

My notes, guides, and templates for building software with **spec-driven development** and AI coding agents (Claude Code).

**Idea:** before writing code, write down *what* to build (`spec.md`), *how* to build it (`plan.md`), and *in what order* (`tasks.md`). Then build it task by task and validate it against the spec.

## Contents

| Path | What it is |
|---|---|
| [`mouredev/fileSamples/SPEC.md`](mouredev/fileSamples/SPEC.md) | **The SDD guide**: the three documents, workflow, EARS requirements, open questions, boundaries, implementation, validation, a full website example, templates, and checklists |
| [`mouredev/fileSamples/PROMPTS.md`](mouredev/fileSamples/PROMPTS.md) | Reusable prompt library: one prompt per SDD step |
| [`mouredev/fileSamples/AGENTS.md`](mouredev/fileSamples/AGENTS.md) | Blank `AGENTS.md` template (rules for AI agents) |
| [`mouredev/startup-web/`](mouredev/startup-web/) | **Project scaffold** ready for Claude Code: `.claude/` config and `/sdd-*` slash commands, `CLAUDE.md`, `AGENTS.md`, constitution, and a first feature spec |

## SDD flow

```
spec.md (what) → clarify → plan.md (how) → tasks.md (steps) → implement → validate
```

## Start a new project

1. Copy `mouredev/startup-web/` and rename it.
2. Edit `AGENTS.md`, `docs/constitution.md`, and `README.md` for the new project.
3. Delete the example in `specs/` and run `claude` inside the folder.
4. Use `/sdd-spec <idea>` and follow the flow.

## Sources

- [MoureDev](https://www.youtube.com/@mouredev) SDD course (YouTube)
- [EARS](https://alistairmavin.com/ears/) — Easy Approach to Requirements Syntax
- [GitHub spec-kit](https://github.com/github/spec-kit)
- [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills)
