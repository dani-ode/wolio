# T001: Establish Rust workspace and crate skeleton

**Status:** `READY` | **Priority:** `P0` | **Milestone:** `M01`

## Objective
Create the minimal buildable Rust workspace and crate boundaries for the
vertical slice, without implementing language behavior.

## Authority
- Product: [`blueprint.md`](../blueprint.md)
- Rules: [`DEPENDENCY_RULES.md`](../01-architecture/DEPENDENCY_RULES.md)
- Milestone: [`M01-VERTICAL_SLICE.md`](../03-implementation/milestones/M01-VERTICAL_SLICE.md)

## Scope
Repository root manifests and only the crates required by M01: lexer, parser,
AST, diagnostics, and CLI. Do not invent public language APIs.

## Acceptance criteria
- [ ] `cargo check --workspace` succeeds
- [ ] `cargo test --workspace` succeeds
- [ ] Dependency direction follows the architecture rules
- [ ] No unrelated files are modified

## Verification
```bash
cargo check --workspace
cargo test --workspace
```

## Dependencies
None. Blocks T002.
