`.blueprint/04-tasks/T001-workspace-skeleton.md`
# T001: Establish Rust workspace and crate skeleton

| Field | Value |
|---|---|
| Milestone | `M01` |
| Priority | `P0` |
| Depends on | owner acceptance of the revision 1.3 baseline |
| Blocks | T002 |
| Acceptance families | common gates (ACCEPTANCE_MATRIX) |
| Bounded areas | repository root manifests, toolchain/CI configuration, M01 crate skeletons |
| Execution status | `.execution/task-status.md` (sole source) |
| Report | `.execution/reports/T001.md` |

## Objective
Create the minimal buildable Rust workspace and crate boundaries for the
vertical slice, without implementing language behavior.

## Authority
- Baseline: [DOCUMENT_AUTHORITY.md](../DOCUMENT_AUTHORITY.md), [BASELINE-V1.md](../BASELINE-V1.md)
- Toolchain: [DELIVERY_POLICY.md](../03-implementation/DELIVERY_POLICY.md)
- Delivery: [IMPLEMENTATION_SPECIFICATION_V1.md](../03-implementation/IMPLEMENTATION_SPECIFICATION_V1.md)
- Acceptance: [ACCEPTANCE_MATRIX.md](../05-quality/ACCEPTANCE_MATRIX.md) common gates, [DEFINITION_OF_DONE.md](../05-quality/DEFINITION_OF_DONE.md)
- Supporting only: [blueprint.md](../blueprint.md), [DEPENDENCY_RULES.md](../01-architecture/DEPENDENCY_RULES.md), [M01-VERTICAL_SLICE.md](../03-implementation/milestones/M01-VERTICAL_SLICE.md)

## Scope
In scope:
- Repository root manifests and only the crates required by M01: lexer, parser,
  AST, diagnostics, and CLI. Do not invent public language APIs.
- `rust-toolchain.toml` with the exact then-current stable Rust version,
  tracked `Cargo.lock`, rustfmt and clippy components.
- GitHub Actions on a pinned Linux runner image running the common gates.

Out of scope (owner):
- Tokenization (T002), grammar/AST (T003), CLI behavior and diagnostics (T004).
- Crates for later milestones; create them only when their task needs them.
- LLVM or any native backend dependency (T051).

## Normative requirements
- Linux x86_64 is the supported V1 platform; no floating tool version in
  recorded release evidence (DELIVERY_POLICY).
- Dependency arrows: parser -> lexer/AST; diagnostics are shared low-level data;
  no dependency cycles; runtime never depends on codegen or LLVM; LLVM is not a
  dependency of frontend/interpreter-only builds (DELIVERY_POLICY).
- Create only useful crates, not every historical crate sketch
  (IMPLEMENTATION_SPECIFICATION_V1).

## Acceptance criteria
- [ ] `cargo check --workspace` succeeds
- [ ] `cargo test --workspace` succeeds
- [ ] Dependency direction follows the architecture rules
- [ ] No unrelated files are modified
- [ ] Exact stable Rust version pinned; rustfmt/clippy installed; Cargo.lock tracked
- [ ] CI runs the common ACCEPTANCE_MATRIX checks on a pinned Linux environment
- [ ] Selected crate layout and Rust version recorded in the report

## Verification
```bash
cargo check --workspace
cargo fmt --all -- --check
cargo clippy --workspace --all-targets -- -D warnings
cargo test --workspace
```
Evidence: `.execution/reports/T001.md`

## Dependencies and open items
- Depends on: owner acceptance recorded in `.execution/baseline-acceptance.md`; blocks: T002.
- Execution prerequisites: Rust toolchain installation on the host.
- Open: none. No language implementation here.
