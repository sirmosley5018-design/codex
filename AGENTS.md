# Repository Guidelines

## Project Structure & Module Organization

`codex-rs/` contains the primary Rust workspace; crates use the `codex-*` package prefix (for example, `codex-rs/core/` is `codex-core`). The terminal UI lives in `codex-rs/tui/`, app-server APIs in `codex-rs/app-server*`, and integration support beside the crates it tests. `codex-cli/` contains the CLI package, `sdk/` contains language SDKs, and `tools/`, `scripts/`, and `bazel/` hold repository automation. CI definitions are under `.github/workflows/`; shared actions and scripts live in `.github/actions/` and `.github/scripts/`.

## Build, Test, and Development Commands

Run repository recipes from the root unless noted:

- `cd codex-rs; cargo build -p codex-cli` builds a specific Rust package.
- `cd codex-rs; just fmt` formats Rust changes.
- `cd codex-rs; just test -p codex-tui` runs a focused package test suite. Do not invoke `cargo test` directly.
- `just argument-comment-lint` checks comments on opaque positional arguments.
- `just bazel-lock-update` refreshes `MODULE.bazel.lock` after Cargo dependency changes.
- `pnpm install` installs repository maintenance tooling; `pnpm format` checks Markdown, JSON, YAML, and JavaScript formatting.

## Coding Style & Naming Conventions

Follow `rustfmt` and Clippy. Use `snake_case` for modules/functions, `UpperCamelCase` for types, and exhaustive `match` expressions when practical. Inline `format!` variables, prefer method references to redundant closures, and avoid ambiguous boolean or `Option` parameters. Keep modules focused; add a module instead of extending files already near 800 lines. Public traits require role and implementation documentation.

## Testing Guidelines

Place new Rust test modules in sibling `*_tests.rs` files. Prefer integration tests for agent behavior and whole-object assertions with `pretty_assertions::assert_eq`. TUI-visible changes require reviewed `insta` snapshots; inspect pending files with `cargo insta pending-snapshots -p codex-tui`. Run the narrowest relevant package tests before broader suites.

## Commits & Pull Requests

Use short, imperative commit subjects such as `Fix Windows cache paths` or scoped forms like `ci: harden workflows`. Keep changes reviewable and normally below 800 lines. Pull requests should explain behavior and risk, link relevant issues, list validation commands, and include screenshots or snapshot evidence for UI changes. Never commit credentials; use GitHub secrets or local environment configuration.

## Agent Execution Policy

Repository work is explicitly read/write. Treat complete, runnable snippets as requested implementation unless clearly labeled illustrative; distinguish them from TODOs, stubs, pseudocode, mocks, and placeholder values. When asked to build, fix, configure, or implement, enact necessary edits and proportionate validation without waiting for follow-up. Continue until complete; do not stop at diagnosis, instructions, suggestions, scaffolding, or a partial patch. Preserve unrelated user changes and report genuine external blockers precisely.
