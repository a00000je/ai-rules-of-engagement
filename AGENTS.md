# AGENTS.md — Rules of Engagement

These rules apply to **everyone** who works in this repository: AI coding assistants
(Claude Code, GitHub Copilot, Cursor, Codex and any other tool that reads `AGENTS.md`)
and human contributors. This file is the single source of truth. `CLAUDE.md`,
`.github/copilot-instructions.md` and `.cursor/rules/engagement.mdc` only point here.

Primary stacks: Python and JavaScript/TypeScript.

---

## This project

> **Edit this section for each project.** It is the only part of this file that
> should differ between projects. Everything below it is shared.

| Item | Value |
| --- | --- |
| Language(s) | _e.g. Python 3.12 / TypeScript 5_ |
| Package manager | _e.g. uv / pnpm_ |
| Formatter | _e.g. Ruff format / Prettier_ |
| Linter | _e.g. Ruff / ESLint_ |
| Type checker | _e.g. Pyright / tsc --noEmit_ |
| Test runner | _e.g. pytest / Vitest_ |
| Where secrets live | _e.g. local `.env` + GitHub Actions secrets, or a secrets manager_ |

Commands (the AI runs these before calling a task done):

```bash
# install
<install command>
# format
<format command>
# lint
<lint command>
# type-check
<type-check command>
# test (full suite)
<test command>
```

---

## 1. AI collaboration

The AI works as a careful pair programmer: it does what was asked, reports what it
changed, and asks before anything hard to undo.

1. **Ask vs. act.** Act directly on clear, reversible requests. Ask first when the
   request is ambiguous or reaches beyond the named scope.
2. **Always ask before** these, even inside the task:
   - Adding, removing or upgrading dependencies
   - Database schema, migrations or data file changes
   - CI workflows, deploy config or infrastructure
   - Deleting or moving files, or renaming public APIs
3. **Stay in scope.** Small tidy-ups in lines already being touched are fine (a typo,
   an unused import). Anything bigger (refactors, renames, reformatting) is suggested
   in the report, not done.
4. **Read before writing.** Follow the patterns already in the codebase before adding
   new ones. Read this file and the README first.
5. **Report every task** in this structure:
   - **Changes:** files and what changed in each
   - **Reasoning:** why this approach
   - **Tests run:** commands and results
   - **Risks:** what could break, assumptions made
   - **Follow-ups:** suggested next steps, including out-of-scope cleanups
6. **Say what you don't know.** Flag guesses, untested paths and assumptions plainly.
   Never invent APIs, file paths or test results.
7. **Verify your own work.** Run the formatter, linter, type checker and tests before
   calling a task done.
8. **Humans decide.** A human reviews and merges. The AI never merges its own pull
   request.

## 2. Code quality & tests

Every change leaves the code typed, tested, documented and linted. These rules name
tool *categories*; the "This project" section above names the actual tools.

| Category | Requirement |
| --- | --- |
| Formatter | One formatter, run automatically; no hand-formatting debates |
| Linter | Runs locally and in CI; new warnings block merge |
| Type checking | Required; strict on new code |
| Test runner | One command runs the full suite |
| Dependency management | Lockfile committed; one package manager per project |

### Definition of done

A change is done only when every box is ticked:

- [ ] Formatter, linter and type checker pass with no new warnings
- [ ] Typed everywhere: type hints on all new Python code; TypeScript (not plain
      JavaScript) for new JS projects; no `any` or `# type: ignore` without a comment
      saying why
- [ ] Unit tests cover new behavior; each bug fix gets a regression test
- [ ] Integration tests cover changes that cross a boundary (API, database, file
      system, external service)
- [ ] All existing tests pass (no coverage percentage is enforced)
- [ ] Every function, public or private, has a docstring or JSDoc; comments explain
      *why*, not what
- [ ] README and docs updated when behavior or setup changes
- [ ] Pull request stays under ~400 changed lines (soft limit); bigger work is split
      into a series of PRs

## 3. Security & secrets

Secrets never enter the repo or the AI's context, and every new dependency is a
deliberate, checked choice.

1. **No secrets in code.** Credentials never appear in source, config or history.
   Each project chooses where secrets live and records it in "This project" above.
   Commit a `.env.example` with placeholder values; `.env` stays in `.gitignore`.
2. **Scan before commit.** A pre-commit secret scanner (gitleaks) runs on every
   commit. A leaked secret is **rotated**, not just deleted.
3. **The AI never touches secrets.** It never opens `.env` files, key files or
   credential stores, and never prints, logs or pastes a secret. If it needs a value,
   it asks the human to set an environment variable.
4. **Dependencies are deliberate.**
   - Before adding a package, the AI states why it's needed, its license and its
     maintenance status, then asks for approval
   - Permissive licenses only (MIT, Apache-2.0, BSD, ISC) without approval; anything
     else needs a human's sign-off
   - Versions pinned with a committed lockfile
   - CI runs `pip-audit` / `npm audit` (or equivalent) and fails on high-severity
     issues
   - Dependabot opens update PRs for outdated and vulnerable packages
5. **Validate input at the edges.** Treat user input, files and API responses as
   untrusted. Use parameterized queries; never build SQL or shell commands from
   strings.
6. **No real personal data in tests.** Fixtures use fake data.

## 4. Git workflow

`main` is always releasable; all work happens on short-lived branches merged through a
reviewed pull request.

- **Branches:** `type/short-description`, e.g. `feat/login-form`, `fix/null-date`.
- **Commits:** [Conventional Commits](https://www.conventionalcommits.org/)
  (`feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:`). One logical change per
  commit.
- **Pull requests:** required for every change, even on solo projects. Small and
  focused (soft limit ~400 changed lines); the description says what, why and how it
  was tested. CI must pass before merge.
- **Merging:** squash merge only, so each PR is one commit on `main`. The PR title
  follows Conventional Commits, since it becomes the commit message.
- **AI attribution:** commits the AI writes carry a `Co-Authored-By:` trailer, and the
  PR template's "AI-assisted" box is ticked.

### Hard stops

For humans and AI alike. Enforced by GitHub branch protection on `main` (see
[`docs/branch-protection.md`](docs/branch-protection.md)).

- No direct commits or pushes to `main`
- No force-push to shared branches
- No rewriting published history
- No committing generated files, build output or secrets
- No skipping hooks or CI (`--no-verify`) without a human's say-so
