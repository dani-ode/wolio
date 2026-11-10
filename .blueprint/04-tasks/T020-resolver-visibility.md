`.blueprint/04-tasks/T020-resolver-visibility.md`
# T020: Resolve modules, imports and public visibility

| Field | Value |
|---|---|
| Milestone | `M02` |
| Priority | `P0` |
| Depends on | T004 |
| Blocks | T021 |
| Acceptance families | SEM (resolution, visibility, cycles) |
| Bounded areas | `crates/wolio-resolver`, semantic support |
| Execution status | `.execution/task-status.md` (sole source) |
| Report | `.execution/reports/T020.md` |

## Objective
Resolve every name in a discovered project to exactly one declaration or
binding, enforcing module uniqueness, import rules, visibility, binding rules
and the V1 cycle bans, with structured diagnostics.

## Authority
- Normative: [CORE_V1.md](../02-specifications/language/CORE_V1.md) (modules, calls, and execution), [INTERFACE_CONTRACTS.md](../02-specifications/INTERFACE_CONTRACTS.md) (project inputs; names; lexical and binding clarifications), [EVIDENCE_MODEL.md](../02-specifications/verification/EVIDENCE_MODEL.md) (code registry)
- Acceptance: ACCEPTANCE_MATRIX `SEM`, DEFINITION_OF_DONE
- Supporting only: [WOLIO-RESOLVER.md](../03-implementation/crates/WOLIO-RESOLVER.md), [WOLIO-SEMANTIC.md](../03-implementation/crates/WOLIO-SEMANTIC.md), [M02-SEMANTIC_CORE.md](../03-implementation/milestones/M02-SEMANTIC_CORE.md)

## Scope
In scope:
- Module table for the compiled package from T004 discovery; test modules
  excluded from ordinary compilation.
- Import resolution, qualified `module::symbol` and lexical local names.
- Binding rules (reserved names, shadowing, duplicates) and the
  operation-to-implementation pairing.
- Import-cycle and recursive call-cycle rejection on the resolved graph.

Out of scope (owner):
- Project discovery and CLI envelopes (T004); types and recursive value types
  (T021); builtin generics and match (T022); persistent `sym_` IDs and query graph
  (T023); `dependencies` list enforcement (T031); test-module inclusion in
  test/verify (T032); package-closure imports (T070).

## Normative requirements
- One flat module per file; module names are unique within the package;
  duplicate module names fail (CORE_V1, INTERFACE_CONTRACTS).
- Imports name modules, not paths. `module::symbol` resolves only imported
  public declarations; local names resolve lexically; no implicit prelude except
  builtins; imports cannot access private declarations (CORE_V1,
  INTERFACE_CONTRACTS).
- Imports may not form cycles. No overloading, dynamic dispatch, closures, loops
  or recursive call cycles in V1 (CORE_V1, INTERFACE_CONTRACTS).
- Builtin type/constructor names and `console` cannot be redeclared; `result` is
  reserved as a binding name; parameters, lets and pattern payloads may not
  shadow visible bindings; duplicate fields, variants and declarations fail
  (INTERFACE_CONTRACTS).
- Each operation has exactly one implementation in the same module (CORE_V1,
  INTERFACE_CONTRACTS).
- Missing, duplicate or inaccessible symbols use `WOLIO-RESOLVE-001`
  (EVIDENCE_MODEL).

## Acceptance criteria
Positive and boundary:
- [ ] Imported public symbols resolve across modules; local names resolve lexically
- [ ] Builtins resolve without an import; nothing else is implicitly visible
- [ ] Test modules do not enter ordinary compilation

Rejection and failure:
- [ ] Duplicate module, declaration, field and variant names are rejected
- [ ] Private and missing symbols, and qualified use without import, are rejected
- [ ] Import cycles and recursive call cycles are rejected
- [ ] Redeclared builtin/`console`, `result` bindings and shadowing are rejected
- [ ] Missing or duplicate implementations are rejected

Completion:
- [ ] Common gates in EXECUTION_CONTRACTS pass
- [ ] No semantics beyond the cited authority; no unrelated changes
- [ ] Report written under the EXECUTION_CONTRACTS report contract

## Verification
```bash
cargo fmt --all -- --check
cargo clippy --workspace --all-targets -- -D warnings
cargo test --workspace
cargo run -p wolio-cli -- check --project <sem-fixture-project> --json
```
Evidence: `.execution/reports/T020.md` with the SEM fixture list.

## Dependencies and open items
- Depends on: T004; blocks: T021.
- Open: the registry names no dedicated code for import or call cycles. Use an
  existing code only if its registered meaning covers the case; otherwise
  record a blocker and request an additive code amendment.

