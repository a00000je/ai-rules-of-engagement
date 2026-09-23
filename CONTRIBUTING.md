# Contributing

The full rules for humans and AI assistants are in [`AGENTS.md`](AGENTS.md). This page
is the short version.

## Workflow

1. Branch from `main`: `feat/short-description`, `fix/short-description`, ...
2. Commit with [Conventional Commits](https://www.conventionalcommits.org/).
3. Open a pull request using the template. Keep it under ~400 changed lines.
4. CI must pass and a human must review. PRs are squash-merged.

## Setup

```bash
pre-commit install   # formatter, linter and secret scan on every commit
cp .env.example .env # then fill in real values locally; never commit .env
```

Project-specific commands are in the "This project" section of `AGENTS.md`.

## Definition of done

Typed, tested (unit + integration where boundaries are crossed), every function
documented, linters clean, docs updated. See `AGENTS.md` §2 for the full checklist.

## Working with AI assistants

AI tools follow the same rules. They ask before changing dependencies, schema, CI or
infrastructure, deleting files or renaming public APIs; they never read secrets; and
they finish each task with a Changes / Reasoning / Tests run / Risks / Follow-ups
report. Tick "AI-assisted" in the PR template when an AI wrote part of the change.
