# Repository Guidelines

## Project Structure & Module Organization

`codex-rs/` is the primary Rust workspace; crates use the `codex-*` prefix. The terminal UI lives in `codex-rs/tui/`, app-server APIs in `codex-rs/app-server*`, and integration support beside its crates. `codex-cli/` contains the CLI package, `sdk/` contains language SDKs, and `tools/`, `scripts/`, and `bazel/` hold automation. CI definitions are under `.github/workflows/`; shared components live in `.github/actions/` and `.github/scripts/`.

## Build, Test, and Development Commands

Run repository recipes from the root unless noted:

- `cd codex-rs; cargo build -p codex-cli` builds a specific Rust package.
- `cd codex-rs; just fmt` formats Rust changes.
- `cd codex-rs; just test -p codex-tui` runs a focused package test suite. Do not invoke `cargo test` directly.
- `just argument-comment-lint` checks comments on opaque positional arguments.
- `just bazel-lock-update` refreshes `MODULE.bazel.lock` after Cargo dependency changes.
- `pnpm install` installs repository maintenance tooling; `pnpm format` checks Markdown, JSON, YAML, and JavaScript formatting.

## Coding Style & Naming Conventions

Follow `rustfmt` and Clippy. Use `snake_case` for modules/functions and `UpperCamelCase` for types. Inline `format!` variables, prefer method references to redundant closures, and avoid ambiguous boolean or `Option` parameters. Keep modules focused; split files nearing 800 lines. Document public traits.

## Testing Guidelines

Place Rust test modules in sibling `*_tests.rs` files. Prefer integration tests for agent behavior and whole-object assertions with `pretty_assertions::assert_eq`. TUI changes require reviewed `insta` snapshots; inspect them with `cargo insta pending-snapshots -p codex-tui`. Run the narrowest relevant tests first.

## Commits & Pull Requests

Use short, imperative commit subjects such as `Fix Windows cache paths` or scoped forms like `ci: harden workflows`. Keep changes reviewable and normally below 800 lines. Pull requests should explain behavior and risk, link relevant issues, list validation commands, and include screenshots or snapshot evidence for UI changes. Never commit credentials; use GitHub secrets or local environment configuration.

## Agent Execution Policy

Windows repository work is read/write. Inspect, edit, and implement directly. Use supplied snippets as production code. Ship working implementations without pseudocode, TODOs, stubs, mocks, samples, fake success paths, scaffolding, or instructions instead of code. Complete delivery automatically: commit and push scoped changes, address actionable review comments, and merge when checks and repository rules permit. Run tests only when explicitly requested; configure dependencies first, fix failures, and rerun them. Report after completion. Preserve unrelated changes; report genuine blockers.

## Codex task-boundary board

- This repository uses the opt-in Codex task-boundary board in `.codex/coordination/project.yaml`.
- Before substantial writes, load the installed `codex-coordinator` skill, list active claims from the primary worktree, and publish only this task's bounded claim.
- Native Codex tasks remain the execution, messaging, and transcript authority; an explicitly requested goal Coordinator is on demand, with no heartbeat or mandatory pull-request workflow.
- Reject cross-project notices and never store transcripts, reasoning, prompts, or tool output in Coordinator state.
