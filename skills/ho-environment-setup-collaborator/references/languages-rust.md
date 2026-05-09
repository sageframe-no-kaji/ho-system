# Languages: Rust

Placeholder. No active Rust projects yet.

This module is imported by project-level CLAUDE.md files when the project uses Rust. It is not loaded globally.

When the first Rust project starts, the `ho-environment-setup-collaborator` skill will interrogate for stack specifics (toolchain, crate management, lint configuration, test harness, target platforms) and fill in this module.

Anticipated universal Ho conventions for Rust:

- `cargo` as the build/test/lint orchestrator
- `clippy` for linting (strict)
- `rustfmt` for formatting
- `cargo test` with coverage measurement (cargo-tarpaulin or equivalent)
- Coverage floor enforced (target ≥90% as in Python; Rust's type system reduces some coverage burden)
- Type errors are compile errors; the type-error discipline applies at compile time
- `Cargo.toml` as central config; `Cargo.lock` committed for binaries, optional for libraries
