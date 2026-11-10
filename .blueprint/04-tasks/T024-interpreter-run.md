`.blueprint/04-tasks/T024-interpreter-run.md`
# T024: Implement the interpreter, runtime and `run` command

| Field | Value |
|---|---|
| Milestone | `M02` |
| Priority | `P0` |
| Depends on | T023 |
| Blocks | T030 |
| Acceptance families | RUN; completes M02 |
| Bounded areas | runtime/interpreter, CLI |
| Execution status | `.execution/task-status.md` (sole source) |
| Report | `.execution/reports/T024.md` |

## Objective
Execute checked, contract-free CORE_V1 programs from the manifest entrypoint
with checked arithmetic and captured console output through `wolio run`.

## Authority
- Normative: [CORE_V1.md](../02-specifications/language/CORE_V1.md), [DELIVERY_POLICY.md](../03-implementation/DELIVERY_POLICY.md) (interpreter first), [INTERFACE_CONTRACTS.md](../02-specifications/INTERFACE_CONTRACTS.md) (CLI and output), [EVIDENCE_MODEL.md](../02-specifications/verification/EVIDENCE_MODEL.md)
- Acceptance: ACCEPTANCE_MATRIX `RUN`, M02 row of DELIVERY_PLAN, DEFINITION_OF_DONE
- Supporting only: [WOLIO-RUNTIME.md](../03-implementation/crates/WOLIO-RUNTIME.md), [WOLIO-CLI.md](../03-implementation/crates/WOLIO-CLI.md)

## Scope
In scope: sequential evaluation of typed semantic expressions, checked Int
arithmetic, values, `console::print`, entrypoint selection, `run` human/JSON
output and exit status, FEATURE rejection of M03 contract clauses.

Out of scope (owner):
- Contract evaluation and guards (T030); static effect/dependency checks (T031);
  `test` (T032); native execution and `build` (T050–T053).

## Normative requirements
- Left-to-right evaluation; `&&`, `||`, `if` and `match` evaluate only the
  selected continuation (CORE_V1).
- Overflow, division by zero and `MIN / -1` are runtime faults
  (`WOLIO-RUNTIME-001`); division truncates toward zero; remainder takes the
  dividend's sign; `MIN % -1` is zero; arithmetic never wraps (CORE_V1,
  INTERFACE_CONTRACTS).
- Values are immutable with no observable identity; immutable storage may be
  shared, initially reference counted (CORE_V1, DELIVERY_POLICY).
- `console::print(String) -> Unit` is the sole host adapter; no clock,
  randomness, environment or network (CORE_V1, DELIVERY_POLICY).
- Entry `main() -> Unit` or `main() -> Int` is selected by `[entrypoint]`, must be
  public; libraries may omit it but `run` requires it (CORE_V1,
  INTERFACE_CONTRACTS).
- Human `run` prints console output then the Int result and newline; JSON adds
  `output` and `result.value` (integer, or null for Unit); exit status follows
  EVIDENCE_MODEL, never the returned Int (INTERFACE_CONTRACTS).
- Faults abort the invocation with diagnostics; a host panic is an internal
  failure (exit 3), not a language Result (CORE_V1, DELIVERY_POLICY).
- Contract clauses requiring M03 produce `WOLIO-FEATURE-001`, not unchecked
  execution (CORE_V1).

## Acceptance criteria
Positive and boundary:
- [ ] Unit and Int entrypoints execute; Int prints as result, not exit code
- [ ] Selected branches only; short-circuiting is observable through faults/output
- [ ] Checked limits: MAX, MIN, truncating division, remainder sign, `MIN % -1`
- [ ] Console output is captured in JSON `output` without corrupting stdout

Rejection and failure:
- [ ] Overflow, divide by zero and `MIN / -1` fault with `WOLIO-RUNTIME-001`
- [ ] Missing or non-public entrypoint fails explicitly
- [ ] Unsupported contract profile fails with `WOLIO-FEATURE-001`

Completion:
- [ ] All M02 DELIVERY_PLAN checks pass: pure examples execute, bad types/cycles
      rejected, cache deletion reproduces the graph, boundary arithmetic faults
- [ ] Common gates in EXECUTION_CONTRACTS pass; no unrelated changes
- [ ] Report written under the EXECUTION_CONTRACTS report contract

## Verification
```bash
cargo fmt --all -- --check
cargo clippy --workspace --all-targets -- -D warnings
cargo test --workspace
cargo run -p wolio-cli -- run --project <run-fixture-project> --json
```
Evidence: `.execution/reports/T024.md` with RUN fixtures and exit codes.

## Dependencies and open items
- Depends on: T023; blocks: T030.
- OPEN: CORE_V1 says "contract clauses requiring M03" without enumerating
  whether `effects`, `dependencies`, `errors` and `intent` clauses count, while
  `console::print` must be declared as a dependency. Confirm the exact set
  against CORE_V1/CONTRACT_DESIGN before implementing the FEATURE rule; if not
  determinable, record a blocker and request an amendment.

