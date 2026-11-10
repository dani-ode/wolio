`.blueprint/04-tasks/T003-minimal-parser-ast.md`
# T003: Implement parser and AST for minimal source slice

| Field | Value |
|---|---|
| Milestone | `M01` |
| Priority | `P0` |
| Depends on | T002 |
| Blocks | T004 |
| Acceptance families | FRONT (grammar cases) |
| Bounded areas | parser and AST crates (`wolio-parser`, `wolio-ast`) |
| Execution status | `.execution/task-status.md` (sole source) |
| Report | `.execution/reports/T003.md` |

## Objective
Parse the accepted vertical-slice tokens into a stable, inspectable AST.

## Authority
- Normative: [CORE_V1.md](../02-specifications/language/CORE_V1.md), [INTERFACE_CONTRACTS.md](../02-specifications/INTERFACE_CONTRACTS.md)
- Acceptance: ACCEPTANCE_MATRIX `FRONT`, DEFINITION_OF_DONE
- Supporting only: [WOLIO-PARSER.md](../03-implementation/crates/WOLIO-PARSER.md), [WOLIO-AST.md](../03-implementation/crates/WOLIO-AST.md), [M01-VERTICAL_SLICE.md](../03-implementation/milestones/M01-VERTICAL_SLICE.md)

## Scope
In scope: every CORE_V1 production, precedence and associativity, AST with spans.

Out of scope (owner):
- CLI and JSON envelopes (T004); name resolution and bindings (T020); typing
  (T021–T022); comment-preserving canonical printing (T072).

## Normative requirements
- Grammar is the CORE_V1 EBNF, grammar version `1`; no implicit semicolon
  insertion, no trailing commas (CORE_V1, INTERFACE_CONTRACTS).
- Precedence low to high: `||`, `&&`, equality, ordering, `+ -`, `* / %`, unary,
  field access; arithmetic/logical operators associate left; comparison
  chaining is rejected (CORE_V1).
- The unary prefix applies after field selection (`-x.value`)
  (INTERFACE_CONTRACTS).
- Parentheses around if conditions and match subjects disambiguate record
  construction from blocks (CORE_V1).
- Integer magnitude `9223372036854775808` is allowed only immediately under
  unary minus; other out-of-range literals are rejected (CORE_V1).
- Unsupported syntax produces a diagnostic, never silent degradation
  (BASELINE-V1). Syntax errors use `WOLIO-SYNTAX-001`.

## Acceptance criteria
- [ ] Empty module and hello-world examples parse
- [ ] Invalid syntax returns deterministic diagnostics
- [ ] AST output has snapshot or equivalent structural tests
- [ ] Every CORE_V1 production and precedence rule has FRONT acceptance coverage
- [ ] Malformed declarations and chained comparisons are rejected
- [ ] Integer literal range boundary (MIN under unary minus) is covered
- [ ] Common gates in EXECUTION_CONTRACTS pass; report written

## Verification
```bash
cargo test -p wolio-parser -p wolio-ast
cargo fmt --all -- --check
cargo clippy --workspace --all-targets -- -D warnings
cargo test --workspace
```
Evidence: `.execution/reports/T003.md`

## Dependencies and open items
- Depends on: T002; blocks: T004.
- Open: none.
