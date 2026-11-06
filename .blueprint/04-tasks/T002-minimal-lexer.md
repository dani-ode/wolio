# T002: Implement lexer for minimal source slice

**Execution status:** see `.execution/task-status.md` | **Priority:** `P0` | **Milestone:** `M01`

## Objective
Tokenize the minimal source forms required by the vertical slice with stable,
structured errors.

## Authority
- Language: [`CORE_V1.md`](../02-specifications/language/CORE_V1.md), `../02-specifications/INTERFACE_CONTRACTS.md`
- Crate plan: [`WOLIO-LEXER.md`](../03-implementation/crates/WOLIO-LEXER.md)
- Milestone: [`M01-VERTICAL_SLICE.md`](../03-implementation/milestones/M01-VERTICAL_SLICE.md)

## Scope
Lexer crate only; no parser or semantic validation.

## Acceptance criteria
- [ ] Valid minimal examples tokenize deterministically
- [ ] Invalid characters produce structured diagnostics
- [ ] Boundary and error cases are tested
- [ ] Full CORE_V1 lexical vocabulary and FRONT lexical acceptance cases pass

## Verification
```bash
cargo test -p wolio-lexer
```

## Dependencies
T001. Blocks T003.
