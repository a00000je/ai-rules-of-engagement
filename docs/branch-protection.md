# Branch protection for `main`

GitHub enforces the hard stops in `AGENTS.md` §4. Branch settings are not copied when a
repo is created from a template, so apply them once per new project.

## Settings → Rules → Rulesets → New branch ruleset

- **Target:** default branch (`main`)
- **Enforcement:** Active
- Turn on:
  - Restrict deletions
  - Block force pushes
  - Require a pull request before merging
    - Allowed merge methods: **Squash** only
  - Require status checks to pass (add your CI job once it exists)

On solo projects, set "Required approvals" to 0 so you can merge your own PR; the PR
itself is still required.

## Settings → General → Pull Requests

- Allow squash merging: **on**, default commit message "Pull request title"
- Allow merge commits: **off**
- Allow rebase merging: **off**
- Automatically delete head branches: **on**

## Settings → Code security

- Dependabot alerts and security updates: **on**
- Secret scanning and push protection: **on** (where available)
