# AI Rules of Engagement — project template

A GitHub template repo that gives every new Python or JavaScript/TypeScript project
one shared set of rules for AI coding assistants and human contributors.

## Use it

1. On GitHub, click **Use this template → Create a new repository**.
2. Fill in the **"This project"** section of `AGENTS.md` (tools, commands, where secrets live).
3. Uncomment the formatter/linter hooks for your stack in `.pre-commit-config.yaml`,
   then run `pre-commit install`.
4. Trim `.github/dependabot.yml` to the ecosystems you use.
5. Apply branch protection: see [`docs/branch-protection.md`](docs/branch-protection.md).
6. Replace this README with your project's own.

For an existing project, copy the files below into it and do steps 2–5.

## What's inside

| File | Purpose |
| --- | --- |
| `AGENTS.md` | The canonical rules. Read by Codex, Cursor and other AGENTS.md-aware tools |
| `CLAUDE.md` | Imports `AGENTS.md` for Claude Code |
| `.github/copilot-instructions.md` | Points GitHub Copilot to `AGENTS.md` |
| `.cursor/rules/engagement.mdc` | Always-on Cursor rule pointing to `AGENTS.md` |
| `CONTRIBUTING.md` | Human-facing summary |
| `.github/pull_request_template.md` | What / why / how tested / checklist / AI-assisted |
| `.github/dependabot.yml` | Automated dependency update PRs |
| `.pre-commit-config.yaml` | Secret scan (gitleaks), hygiene hooks, slots for formatter/linter |
| `.env.example`, `.gitignore` | Placeholder secrets; real `.env` never committed |
| `docs/branch-protection.md` | One-time GitHub settings that enforce the hard stops |

## Changing the rules

Edit `AGENTS.md` here (via a PR). The pointer files never need to change. Projects
created earlier keep their own copy, so port important rule changes to them.
