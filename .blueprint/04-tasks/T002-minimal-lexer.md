`.blueprint/04-tasks/T002-minimal-lexer.md`
# T002: Implement lexer for minimal source slice

| Field | Value |
|---|---|
| Milestone | `M01` |
| Priority | `P0` |
| Depends on | T001 |
| Blocks | T003 |
| Acceptance families | FRONT (lexical cases) |
| Bounded areas | lexer crate (`wolio-lexer`), shared diagnostic values |
| Execution status | `.execution/task-status.md` (sole source) |
| Report | `.execution/reports/T002.md` |

## Objective
Tokenize the minimal source forms required by the vertical slice with stable,
structured errors.

## Authority
- Normative: [CORE_V1.md](../02-specifications/language/CORE_V1.md), [INTERFACE_CONTRACTS.md](../02-specifications/INTERFACE_CONTRACTS.md) (lexical and binding clarifications), [EVIDENCE_MODEL.md](../02-specifications/verification/EVIDENCE_MODEL.md) (spans, codes)
- Acceptance: ACCEPTANCE_MATRIX `FRONT`, DEFINITION_OF_DONE
- Supporting only: [WOLIO-LEXER.md](../03-implementation/crates/WOLIO-LEXER.md), [M01-VERTICAL_SLICE.md](../03-implementation/milestones/M01-VERTICAL_SLICE.md)

## Scope
In scope: the complete CORE_V1 lexical vocabulary in the lexer crate only.

Out of scope (owner):
- Grammar, precedence and integer-literal range in context (T003).
- CLI output envelopes (T004); semantic validation (T020 onward).

## Normative requirements
- Source is UTF-8; invalid UTF-8 is rejected (CORE_V1, ACCEPTANCE_MATRIX).
- Identifiers are ASCII `[A-Za-z_][A-Za-z0-9_]*`, case sensitive; every quoted
  alphabetic grammar terminal is a keyword reserved in every position
  (CORE_V1, INTERFACE_CONTRACTS).
- `//` comments end at newline; no block comments. CRLF and LF are whitespace
  (INTERFACE_CONTRACTS).
- Strings use JSON escapes; unescaped control characters and unpaired surrogate
  escapes are rejected; JSON surrogate pairs decode to one Unicode scalar
  (INTERFACE_CONTRACTS).
- Integer literals are decimal digits without separators; unary minus is an
  operator, not part of the literal (CORE_V1).
- Spans are zero-based original UTF-8 byte offsets, start inclusive, end
  exclusive (EVIDENCE_MODEL, INTERFACE_CONTRACTS). Invalid tokens use
  `WOLIO-SYNTAX-001`.

## Acceptance criteria
- [ ] Valid minimal examples tokenize deterministically
- [ ] Invalid characters produce structured diagnostics
- [ ] Boundary and error cases are tested
- [ ] Full CORE_V1 lexical vocabulary and FRONT lexical acceptance cases pass
- [ ] Unicode strings and CRLF inputs yield correct byte spans
- [ ] Invalid UTF-8, invalid token, invalid escape and unfinished string are rejected
- [ ] Common gates in EXECUTION_CONTRACTS pass; report written

## Verification
```bash
cargo test -p wolio-lexer
cargo fmt --all -- --check
cargo clippy --workspace --all-targets -- -D warnings
cargo test --workspace
```
Evidence: `.execution/reports/T002.md`

## Dependencies and open items
- Depends on: T001; blocks: T003.
- Open: none.
