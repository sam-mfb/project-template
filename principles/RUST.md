# Rust

This document says what [ARCHITECTURE.md](ARCHITECTURE.md) and [CODING.md](CODING.md) mean for a project written in Rust. A module from those documents is a crate.

1. A module is a crate. The repository is a Cargo workspace with one crate per module.

2. Every dependency's version and features are declared once, at the workspace level, and each crate takes what it uses from there.

3. The toolchain is pinned in `rust-toolchain.toml`, and builds use the lockfile.

4. Visibility is the module boundary. An item is `pub` only when another crate needs it.

5. A dependency is a trait the consuming crate declares, with an implementation supplied at startup.

6. Code that touches the machine is behind a `real` feature, and its stand-in is behind a `testing` feature. Only the program turns `real` on; only tests turn `testing` on.

7. A third-party crate is imported by one crate, and its types stay inside that crate.

8. Errors are typed with `thiserror`. Program code returns `Result`; `unwrap` and `expect` are for tests.

9. A lint exception uses `#[expect(..., reason = "...")]`, never `#[allow]`.

10. Logging uses `tracing` and happens only in side-effect and composition crates.

11. `cargo test`, `cargo clippy --all-targets -- -D warnings`, and `cargo fmt --check` MUST be clean before every commit and in CI.

12. Unit tests sit beside the code they test. Integration tests live in `tests/`.
