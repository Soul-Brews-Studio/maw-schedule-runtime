# maw-schedule-runtime agent contract

This workspace owns the filesystem and process adapters around the pure
`maw-schedule` contract. Keep the two adapter boundaries explicit, injected,
and fail-closed.

- Rust edition 2021; `unsafe_code` is forbidden.
- Clippy pedantic, `unwrap_used`, and `expect_used` warnings are errors in CI.
- Keep `maw-schedule` on one exact full-SHA workspace dependency. Its types are
  part of both public APIs, so consumers must resolve the same Cargo identity.
- Preserve argv-only `launchctl` execution behind `LaunchctlRunner`; never use a
  shell or run destructive live launchd operations in tests.
- Keep schedule-runner filesystem locking, atomic writes, secret handling, and
  output validation fail-closed. Tests must never use real secrets.
- CI supports Linux and macOS. Do not claim live `launchctl` integration unless
  a non-mutating test actually invokes it.
- Run the README verification commands before opening a PR.
- Open changes through a branch and PR; do not push implementation changes
  directly to `main`.
- Never add or commit `ψ/`.
