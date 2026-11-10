# ruby-ci-workflow

Reusable GitHub Actions workflows for my Ruby gems.

| Workflow | Purpose |
|---|---|
| `ci.yml` | Ruby 3.2/3.3/3.4 on Linux, 3.4 on Windows + macOS, gem build, SonarQube, `CI passed` gate |
| `release.yml` | Tag + GitHub Release from the gemspec/CHANGELOG, publish to GitHub Packages |

This repo is Ruby only. Other languages get their own repo (`rust-ci-workflow`, ...) with the same
file names (`ci.yml`, `release.yml`), so only the repo name in the `uses:` line changes.
Reusable workflows must live directly in `.github/workflows/`, no subfolders.

## Use it in a project repo

Copy the files from `examples/caller-repo/.github/` into the project's `.github/` folder.
Each project also needs:

- `sonar-project.properties` (its own project key)
- a `SONAR_TOKEN` repository secret
- exactly one `*.gemspec` and a `CHANGELOG.md` in the repo root
- required status check `CI passed` in its ruleset (the caller `ci.yml` has a `CI passed` gate job)

## Maintaining this repo

- Dependabot bumps third-party actions here, `actionlint` validates the PR, auto-merge merges it.
- Every push to `main` moves the `v1` tag (`update-tag.yml`).
- Repo settings: enable **Allow auto-merge**, and require the `actionlint` check on `main`.
- Breaking changes: create `v2`, copy `update-tag.yml` to move `v2`, and migrate callers one by one.
