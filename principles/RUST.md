# Rust

This document says what [ARCHITECTURE.md](ARCHITECTURE.md) and [CODING.md](CODING.md) mean for a project written in Rust. A module from those documents is a crate.

1. A module is a crate. The repository is a Cargo workspace with one crate per module.

2. Every dependency's version and features are declared once, at the workspace level, and each crate takes what it uses from there.

3. The toolchain is pinned in `rust-toolchain.toml`, and builds use the lockfile.

4. Visibility is the module boundary. An item is `pub` only when another crate needs it.

5. A dependency is a trait, declared by the consuming crate or by a contract crate, with an implementation supplied at startup.

6. A side-effect crate exports only its implementation of a contract declared elsewhere. The crate that declares the contract provides the stand-in behind a `testing` feature. Only the program depends on side-effect crates; only tests turn `testing` on.

7. A third-party crate is imported by one crate, and its types stay inside that crate. This rule does not apply to third-party crates that shape the language (e.g., thiserror, futures, serde).

8. Errors are typed with `thiserror`. Program code returns `Result`; `unwrap` and `expect` are for tests.

9. A lint exception uses `#[expect(..., reason = "...")]`, never `#[allow]`.

10. Logging uses `tracing` and happens only in side-effect and composition crates.

11. `cargo test`, `cargo clippy --all-targets -- -D warnings`, and `cargo fmt --check` MUST be clean before every commit and in CI.

12. Unit tests sit beside the code they test. Integration tests live in `tests/`.
