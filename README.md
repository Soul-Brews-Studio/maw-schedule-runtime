# maw-schedule-runtime

Filesystem and process adapters for the maw schedule contract.

This workspace is extracted from
[`Soul-Brews-Studio/maw-rs`](https://github.com/Soul-Brews-Studio/maw-rs)
and contains two unpublished crates:

- `maw-schedule-launchd`: plist rendering and launchd synchronization through
  an injected argv-only `LaunchctlRunner`;
- `maw-schedule-runner`: locked run-state storage, job execution, secret
  lookup, logging, and output validation.

These are explicit I/O adapters, not side-effect-free core crates. Their shared
contract lives in `maw-schedule` from
[`Soul-Brews-Studio/maw-crates`](https://github.com/Soul-Brews-Studio/maw-crates).
Both adapters expose `maw-schedule` types publicly, so consumers that also use
that crate must resolve the same Git URL and exact revision.

## Platform boundary

The workspace is verified on Linux and macOS. `SystemLaunchctl` executes only
on macOS and returns an unsupported-platform error elsewhere. The launchd state
machine tests use an injected fake; the macOS CI job compiles the concrete
adapter but does not invoke live `launchctl` or mutate launchd state.

## Use

Consumers use both unpublished crates as Cargo git dependencies pinned to one
immutable full commit SHA.

## Verify

```bash
cargo fmt --all -- --check
cargo test --workspace --locked --no-fail-fast
cargo clippy --workspace --all-targets --locked -- -D warnings
```

CI runs the same commands on both Ubuntu and macOS with Rust 1.97.1.

## Provenance

The initial crate source and tests were copied from `maw-rs` alpha at
`6d683c4556c6fa73ce32830a5da915760bfb18de`. Production source and tests are
byte-identical; the member manifests only lift the shared `maw-schedule` pin to
this workspace root so the two public APIs cannot drift onto different Cargo
identities.
