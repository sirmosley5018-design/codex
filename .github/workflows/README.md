# Workflow Strategy

The active fork workflows provide focused Windows and Linux verification for pull requests and `main`.

## Pull Requests

Forks run the same blocking checks without depending on artifacts or credentials
owned by `openai/codex`. Maintainers can also start `blocking-ci` manually from
the Actions page when validating repository configuration.

- `bazel.yml` is the main pre-merge verification path for Rust code.
  It runs Bazel `test` and Bazel `clippy` on the supported Bazel targets,
  including the generated Rust test binaries needed to lint inline `#[cfg(test)]`
  code.
- `rust-ci.yml` keeps the Cargo-native PR checks intentionally small:
  - `cargo fmt --check`
  - `cargo shear`
  - `argument-comment-lint` on Linux and Windows
  - `tools/argument-comment-lint` package tests when the lint or its workflow wiring changes

## Main Branch

`blocking-ci.yml` also runs on pushes to `main`, keeping the merge gate and
post-merge verification identical. The redundant post-merge workflow is not
used in this fork.

## Rule Of Thumb

- If a build/test/clippy check can be expressed in Bazel, prefer putting the PR-time version in `bazel.yml`.
- Keep `rust-ci.yml` fast enough that it usually does not dominate PR latency.
