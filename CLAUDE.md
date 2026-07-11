# Project Requirements — RokketWrapper

## What This Is

A VS Code extension (`rokketek-wrapper`) that wraps the Claude Code CLI and OpenAI Codex
CLI in a webview GUI, with a Telegram remote-control bridge and voice-input transcription.
See [README.md](README.md) for features/architecture and [PROJECT.md](PROJECT.md) for the
fork-origin history (forked from `RokketGSD/gsd-vscode`).

## Repository and Publisher Identity

The project's GitHub home moved from the `Kile-Thomson` account to **`asror-z`**. Every
reference to the repo owner (`package.json`'s `repository.url`, `update-checker.ts`'s
`GITHUB_OWNER` constant, `release.yml`'s CI git-commit identity) must name `asror-z` —
never re-derive or "fix" this back to `Kile-Thomson` from an old release/tag found on
GitHub. A live GitHub API check during a past session found real published releases (up
to v0.1.24) still sitting under `Kile-Thomson/Rokket-Wrapper` with `asror-z/Rokket-Wrapper`
returning 404 — that reflects the *old* pre-move history, not the current source of truth.
**When judging which account is correct, ask the user rather than trusting whichever
account happens to have GitHub Releases already published** — release history lags an
account migration.

## Versioning — No Git Tags, CI Bumps the Version Itself

This repo does **not** use git tags for versioning (confirmed: `git tag --list` is empty,
`core.hooksPath` is unset). Version bumps happen entirely inside
`.github/workflows/release.yml`'s own CI job: on every push to `master` (except a commit
whose message starts with `chore: bump version`), CI bumps `package.json`'s patch version,
commits that bump itself, builds the VSIX, and creates a GitHub Release (not a bare tag) —
`gh release create "v${VERSION}" ... *.vsix`. When committing a change here via
`smarts-git-automate`, **do not** manually bump the version or create a git tag — CI owns
that step after the push lands. This is a documented exception to that skill's normal
"every version-tracked repo gets one git tag per commit" rule, since this repo's own CI
already performs the equivalent via GitHub Releases instead of tags.

## npm Scripts

`compile` is an alias for `build` (added so the command documented in `README.md`'s
Building section and `CONTRIBUTING.md`'s PR checklist actually resolves). Keep both names
working — don't remove `compile` even if `build` is renamed later; update the alias instead.

## Coverage Gate

`vitest.config.ts`'s coverage thresholds (`lines: 44, statements: 43, functions: 38,
branches: 36`) are calibrated a small safety margin below a real measured baseline (44 test
files / 974 tests, all passing, measured via `npm run test:coverage`), not an arbitrary
round number. When re-calibrating after adding real coverage, re-measure with the same
command rather than guessing a new round number.

## CLI Permission Model

Both `ClaudeCodeProvider` and `CodexProvider` always spawn their CLI with all approval/
sandbox prompts disabled (`--dangerously-skip-permissions` / `--dangerously-bypass-approvals-and-sandbox`)
— this is intentional and documented in [SECURITY.md](SECURITY.md)'s "CLI Permission
Model" section. There is currently no settings-gated escape hatch to run without these
flags; see `roadma/Rokket Wrapper Roadmap 2.md` (local, gitignored) for the open decision
on whether to add one.

## Professionalization Roadmap

A `roadma/` folder (gitignored, per `smarts-improve-roadmap`'s convention) holds
increment-versioned professionalization roadmaps for this project. It is local-only —
never committed, never referenced as a source of truth in this file beyond this pointer.
