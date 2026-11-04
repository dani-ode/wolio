# T003: Implement parser and AST for minimal source slice

**Status:** `READY` | **Priority:** `P0` | **Milestone:** `M01`

## Objective
Parse the accepted vertical-slice tokens into a stable, inspectable AST.

## Authority
- Language: [`LANGUAGE_SPEC.md`](../02-specifications/language/LANGUAGE_SPEC.md)
- Crates: [`WOLIO-PARSER.md`](../03-implementation/crates/WOLIO-PARSER.md), [`WOLIO-AST.md`](../03-implementation/crates/WOLIO-AST.md)
- Milestone: [`M01-VERTICAL_SLICE.md`](../03-implementation/milestones/M01-VERTICAL_SLICE.md)

## Acceptance criteria
- [ ] Empty module and hello-world examples parse
- [ ] Invalid syntax returns deterministic diagnostics
- [ ] AST output has snapshot or equivalent structural tests

## Verification
```bash
cargo test -p wolio-parser -p wolio-ast
```

## Dependencies
T002. Blocks T004.
