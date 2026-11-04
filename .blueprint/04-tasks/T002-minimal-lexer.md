# T002: Implement lexer for minimal source slice

**Status:** `READY` | **Priority:** `P0` | **Milestone:** `M01`

## Objective
Tokenize the minimal source forms required by the vertical slice with stable,
structured errors.

## Authority
- Language: [`LANGUAGE_SPEC.md`](../02-specifications/language/LANGUAGE_SPEC.md)
- Crate plan: [`WOLIO-LEXER.md`](../03-implementation/crates/WOLIO-LEXER.md)
- Milestone: [`M01-VERTICAL_SLICE.md`](../03-implementation/milestones/M01-VERTICAL_SLICE.md)

## Scope
Lexer crate only; no parser or semantic validation.

## Acceptance criteria
- [ ] Valid minimal examples tokenize deterministically
- [ ] Invalid characters produce structured diagnostics
- [ ] Boundary and error cases are tested

## Verification
```bash
cargo test -p wolio-lexer
```

## Dependencies
T001. Blocks T003.
