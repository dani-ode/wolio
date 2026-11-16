`.blueprint/04-tasks/T044-repair-finalize-review-analyzer.md`
# T044: Implement bounded repair, `finalize`, review disposition and anti-workaround analyzer

| Field | Value |
|---|---|
| Milestone | `M04` |
| Priority | `P0` |
| Depends on | T043 |
| Blocks | T060, T070 |
| Acceptance families | AGENT (repair, finalize, review); ANALYZER; completes M04 |
| Bounded areas | agent (`crates/wolio-agent`), `crates/wolio-semantic` (analyzer submodule), verification (`crates/wolio-verify`) |
| Execution status | `.execution/task-status.md` (sole source) |
| Report | `.execution/reports/T044.md` |

## Objective
Enforce all five V1 anti-workaround analyzer rules (`WOLIO-ANALYZER-001`..`005`),
integrate the six hyper-strict linter rules (`WOLIO-LINT-001`..`006`),
implement `finalize` evidence binding, manage the `POST_REVIEW` owner
disposition lifecycle, and enforce the bounded 3-attempt repair policy,
completing M04.

## Authority
- Normative: [LOCAL_PROTOCOL_V1.md](../02-specifications/protocol/LOCAL_PROTOCOL_V1.md), [MUTATION_SCHEMA_V1.md](../02-specifications/protocol/MUTATION_SCHEMA_V1.md), [APPROVAL_MATRIX_V1.md](../02-specifications/protocol/APPROVAL_MATRIX_V1.md), [DIAGNOSTIC_SPEC.md](../02-specifications/verification/DIAGNOSTIC_SPEC.md) (6-tier hierarchy, Tier 4 `ANTI_HACKING_ANALYZER`, Tier 5 `HYPER_STRICT_LINT`, and `WOLIO-LINT-001`..`006`), [EVIDENCE_MODEL.md](../02-specifications/verification/EVIDENCE_MODEL.md), [CORE_V1.md](../02-specifications/language/CORE_V1.md) (Zero-Unsafe Invariant)
- Taxonomy: [AI_BUG_TAXONOMY.md](../06-project-management/AI_BUG_TAXONOMY.md) (`W01`–`W16`)
- Decision: [ADR-007](../01-architecture/decisions/ADR-007-deterministic-identity-and-gated-automation.md), [ADR-008](../01-architecture/decisions/ADR-008-multi-version-anti-workaround-roadmap.md)
- Acceptance: [ACCEPTANCE_MATRIX.md](../05-quality/ACCEPTANCE_MATRIX.md) `AGENT` and `ANALYZER`, M04 row of [DELIVERY_PLAN.md](../06-project-management/DELIVERY_PLAN.md), [DEFINITION_OF_DONE.md](../05-quality/DEFINITION_OF_DONE.md)

## Self-contained implementation context
- **Enforceable V1 Analyzer Rules (`crates/wolio-semantic` analyzer submodule, run after `T031` static checks pass):**
  1. `WOLIO-ANALYZER-001` (`error`): Rejects any mutation request or directive attempting diagnostic suppression, rule disabling, or verification bypass. Also rejects any attempt to introduce `unsafe` blocks, raw pointer syntax, manual memory management constructs, or in-source suppression directives (`#[allow(...)]`, `// nolint`, or equivalent) per the Zero-Unsafe Invariant (CORE_V1). Repair `classes` never include `"suppress"` or `"ignore"`.
  2. `WOLIO-ANALYZER-002` (`error`): Inside an `implementation` of an operation returning `Result<T, E>`, rejects a `match` over a callee `Result` whose `Result::Err(_)` arm ignores/discards its bound error payload and returns a constant/literal `Result::Ok(...)` (swallowed error substitution).
  3. `WOLIO-ANALYZER-003` (`error`): Rejects declared bloat where an operation declares a symbol in `dependencies [...]` that is never called in its `implementation`, or declares an effect in `effects [...]` that is neither directly nor transitively produced by its called operations.
  4. `WOLIO-ANALYZER-004` (`error`): Rejects any literal **or constant-folded pure expression** (e.g., `99 + 1` evaluating to `100`, or `-100`) in a production `implementation` AST matching `wolio.toml` `[analysis]` `forbidden_int_literals` or `forbidden_string_literals`. Tests, contract predicates, and `intent` strings are excluded.
  5. `WOLIO-ANALYZER-005` (`warning`): Emitted once per construct when (a) a `match` has normalized arm bodies that are structurally identical after bound-variable alpha-renaming, OR (b) a production `if`/`match` condition compares a parameter against an exact non-boundary literal (`!= -1, 0, 1`) that appears as a test argument in a `test` block for that operation (test-literal mirroring). Warnings do not fail `verify` or raise approval tier.
- **Hyper-Strict Linter Integration (`DIAGNOSTIC_SPEC` Tier 5, `WOLIO-LINT-001`..`006`):**
  The following `error`-severity lint rules are enforced by the semantic analyzer
  after `T031` static checks pass, with zero in-source suppression (any bypass
  attempt triggers `WOLIO-ANALYZER-001`):
  - `WOLIO-LINT-001`: Unused binding, parameter, or import.
  - `WOLIO-LINT-002`: Dead code or redundant match arm (provably unreachable).
  - `WOLIO-LINT-003`: Tautological contract clause (`requires true`, `x == x`).
  - `WOLIO-LINT-004`: Naked magic literal in business logic (outside nominal constructors and `0`, `1`).
  - `WOLIO-LINT-005`: Missing `intent` clause on a public `operation`.
  - `WOLIO-LINT-006`: Non-`Unit` statement expression discard (also enforced as `WOLIO-TYPE-001` by T021).

## Scope
In scope:
- Analyzer pass (`WOLIO-ANALYZER-001`..`005`) integrated into `verify` and `agent` staging, including Zero-Unsafe Invariant enforcement (W16) under `WOLIO-ANALYZER-001`.
- Hyper-strict linter pass (`WOLIO-LINT-001`..`006`) integrated into `verify`, with all six codes at `error` severity and zero in-source suppression.
- `finalize` protocol method binding complete verification evidence to the published snapshot.
- Owner review disposition (`pending -> accepted` / `pending -> rejected`), self-review prohibition, local pending-review query output, and release/baseline gating on pending or rejected reviews.
- Bounded repair workflow (default maximum 3 attempts) with repair hints that never include `suppress` or `ignore` or automatically weaken contracts.

Out of scope (owner):
- V2 loop-invariant analyzer checks (`T212`).

## Normative requirements
- `WOLIO-ANALYZER-001`..`004` are `error` diagnostics blocking `verify`, `apply`, and `finalize`; `WOLIO-ANALYZER-005` is a `warning` diagnostic that does not fail `verify` or raise approval tier (DIAGNOSTIC_SPEC, APPROVAL_MATRIX_V1).
- `WOLIO-LINT-001`..`006` are `error` diagnostics (Tier 5 `HYPER_STRICT_LINT`) blocking `verify`, with zero in-source suppression; any attempt to suppress triggers `WOLIO-ANALYZER-001` (DIAGNOSTIC_SPEC, CORE_V1).
- `finalize` binds complete technical evidence to the published snapshot; any failed, missing, `not_run`, or required `unknown` obligation blocks `apply` and `finalize` without recording successful evidence (LOCAL_PROTOCOL_V1, APPROVAL_MATRIX_V1, EVIDENCE_MODEL).
- `POST_REVIEW` records allow local build/test/finalize while `pending`, but block release artifacts and baseline acceptance until an owner records `accepted`. Agents cannot resolve their own review (`pending -> accepted` or `rejected`). `pending -> rejected` requires a fresh repair or revert plan against the current snapshot and never silently rolls back later work (LOCAL_PROTOCOL_V1, APPROVAL_MATRIX_V1).
- Repair is a new plan subject to all scope/tier/verification rules with a default budget of 3 attempts before reporting unresolved diagnostics (LOCAL_PROTOCOL_V1).

## Acceptance criteria
Positive and boundary:
- [ ] Unconfigured literals pass; literals in tests, contract predicates, and `intent` are excluded from `WOLIO-ANALYZER-004`
- [ ] Structurally identical normalized `match` arms and test-literal mirroring emit one `WOLIO-ANALYZER-005` warning while allowing `verify`, `apply`, and `finalize` to succeed
- [ ] `AUTO` and `POST_REVIEW` plans finalize when all required gates pass; pending reviews are visible in local queries and allow local builds
- [ ] Owner `accepted` disposition clears release/baseline gating for a `POST_REVIEW` record

Rejection and failure:
- [ ] Suppression/bypass requests fail with `WOLIO-ANALYZER-001`
- [ ] Attempts to introduce `unsafe`, raw pointers, or in-source suppression directives (`#[allow]`, `nolint`) fail with `WOLIO-ANALYZER-001` (W16)
- [ ] Swallowed `Result::Err` returning constant `Result::Ok` fails with `WOLIO-ANALYZER-002`
- [ ] Unused declared `dependencies` or `effects` entries fail with `WOLIO-ANALYZER-003`
- [ ] Configured `forbidden_int_literals` / `forbidden_string_literals` (both raw literals, negative literals, and constant-folded pure expressions like `99 + 1`) fail with `WOLIO-ANALYZER-004` and block `apply`
- [ ] Unused bindings/imports (`WOLIO-LINT-001`), dead code (`WOLIO-LINT-002`), tautological contracts (`WOLIO-LINT-003`), naked magic literals (`WOLIO-LINT-004`), missing `intent` (`WOLIO-LINT-005`), and non-`Unit` discard (`WOLIO-LINT-006`) all fail as `error` diagnostics blocking `verify`
- [ ] Agent self-review is rejected; `pending` or `rejected` review records block release eligibility and baseline acceptance
- [ ] Rejected review disposition requires a new plan against the current snapshot rather than silent rollback
- [ ] Failed, `not_run`, or required `unknown` obligations block `finalize`; exhausting the 3-attempt repair budget stops and reports unresolved diagnostics

Completion:
- [ ] All M04 DELIVERY_PLAN and ACCEPTANCE_MATRIX (`AGENT` and `ANALYZER`) gates pass
- [ ] Common gates in EXECUTION_CONTRACTS pass; no unrelated changes
- [ ] Report written under the EXECUTION_CONTRACTS report contract

## Verification
```bash
cargo fmt --all -- --check
cargo clippy --workspace --all-targets -- -D warnings
cargo test --workspace
```
Evidence: `.execution/reports/T044.md` with AGENT repair/finalize/review and ANALYZER `001`–`005` fixtures.

## Dependencies and open items
- Depends on: T043; blocks: T060, T070.
- Open: none.
