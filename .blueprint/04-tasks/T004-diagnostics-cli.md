# T004: Add deterministic diagnostics and CLI check

**Execution status:** see `.execution/task-status.md` | **Priority:** `P0` | **Milestone:** `M01`

## Objective
Expose parsing through the CLI and return machine-readable diagnostics for
valid and invalid source files.

## Authority
- Shared CLI: `../02-specifications/INTERFACE_CONTRACTS.md`
- Evidence: `../02-specifications/verification/EVIDENCE_MODEL.md`
- Architecture: [`SYSTEM_ARCHITECTURE.md`](../01-architecture/SYSTEM_ARCHITECTURE.md)
- Diagnostics: [`DIAGNOSTIC_SPEC.md`](../02-specifications/verification/DIAGNOSTIC_SPEC.md)
- Crate plan: [`WOLIO-CLI.md`](../03-implementation/crates/WOLIO-CLI.md)

## Acceptance criteria
- [ ] `wolio check` succeeds for valid input
- [ ] Invalid input has stable error code, span, and message
- [ ] CLI exit status distinguishes success and failure
- [ ] init creates a valid project without overwrites; check supports JSON
- [ ] Semantic checks not implemented at M01 are reported not_run

## Verification
```bash
cargo test --workspace
cargo run -p wolio-cli -- check tests/conformance/front/valid/empty.wol --json
```

## Dependencies
T003. Blocks M02.
