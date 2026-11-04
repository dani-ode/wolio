# T004: Add deterministic diagnostics and CLI check

**Status:** `READY` | **Priority:** `P0` | **Milestone:** `M01`

## Objective
Expose parsing through the CLI and return machine-readable diagnostics for
valid and invalid source files.

## Authority
- Architecture: [`SYSTEM_ARCHITECTURE.md`](../01-architecture/SYSTEM_ARCHITECTURE.md)
- Diagnostics: [`DIAGNOSTIC_SPEC.md`](../02-specifications/verification/DIAGNOSTIC_SPEC.md)
- Crate plan: [`WOLIO-CLI.md`](../03-implementation/crates/WOLIO-CLI.md)

## Acceptance criteria
- [ ] `wolio check` succeeds for valid input
- [ ] Invalid input has stable error code, span, and message
- [ ] CLI exit status distinguishes success and failure

## Verification
```bash
cargo test --workspace
cargo run -p wolio-cli -- check <fixture>
```

## Dependencies
T003. Blocks M02.
