`.blueprint/04-tasks/T044-repair-finalize-review-analyzer.md`
# T044: Implement bounded repair, `finalize`, review disposition and analyzer policy

| Field | Value |
|---|---|
| Milestone | `M04` |
| Priority | `P0` |
| Depends on | T043 |
| Blocks | T060, T070 |
| Acceptance families | AGENT (repair, finalize, review); ANALYZER; completes M04 |
| Bounded areas | agent (`crates/wolio-agent`), analyzer (`crates/wolio-analyzer`), verification (`crates/wolio-verify`) |
| Execution status | `.execution/task-status.md` (sole source) |
| Report | `.execution/reports/T044.md` |

## Objective
Enforce the V1 anti-workaround analyzer rules (`WOLIO-ANALYZER-001`..`005`),
implement `finalize` evidence binding, manage the `POST_REVIEW` owner
disposition lifecycle and enforce the bounded 3-attempt repair policy,
completing M04.

## Authority
- Normative: [LOCAL_PROTOCOL_V1.md](../02-specifications/protocol/LOCAL_PROTOCOL_V1.md), [MUTATION_SCHEMA_V1.md](../02-specifications/protocol/MUTATION_SCHEMA_V1.md), [APPROVAL_MATRIX_V1.md](../02-specifications/protocol/APPROVAL_MATRIX_V1.md), [DIAGNOSTIC_SPEC.md](../02-specifications/verification/DIAGNOSTIC_SPEC.md), [EVIDENCE_MODEL.md](../02-specifications/verification/EVIDENCE_MODEL.md)
- Decision: [ADR-007](../01-architecture/decisions/ADR-007-deterministic-identity-and-gated-automation.md)
- Acceptance: [ACCEPTANCE_MATRIX.md](../05-quality/ACCEPTANCE_MATRIX.md) `AGENT` and `ANALYZER`, M04 row of [DELIVERY_PLAN.md](../06-project-management/DELIVERY_PLAN.md), [DEFINITION_OF_DONE.md](../05-quality/DEFINITION_OF_DONE.md)
- Supporting only: [WOLIO-AGENT.md](../03-implementation/crates/WOLIO-AGENT.md), [WOLIO-ANALYZER.md](../03-implementation/crates/WOLIO-ANALYZER.md), [WOLIO-VERIFY.md](../03-implementation/crates/WOLIO-VERIFY.md), [M04-AGENT_PROTOCOL.md](../03-implementation/milestones/M04-AGENT_PROTOCOL.md)

## Scope
In scope:
- Analyzer pass after static checks and before evidence aggregation:
  `WOLIO-ANALYZER-001` (suppression/bypass rejection), `WOLIO-ANALYZER-004`
  (manifest `[analysis]` `forbidden_int_literals` and
  `forbidden_string_literals` in production implementation ASTs), and
  `WOLIO-ANALYZER-005` (warning on structurally identical normalized match-arm
  bodies after bound-variable renaming), without duplicating `WOLIO-ERROR-001`,
  `WOLIO-DEPENDENCY-001` or `WOLIO-EFFECT-001` for `002`/`003`.
- `finalize` protocol method binding complete verification evidence to the
  published snapshot.
- Owner review disposition (`pending -> accepted` / `pending -> rejected`),
  self-review prohibition, local pending-review query output, and release/
  baseline gating on pending or rejected reviews.
- Bounded repair workflow (default maximum 3 attempts) with repair hints that
  never include `suppress` or `ignore` or automatically weaken contracts.

Out of scope (owner):
- Advisory post-V1 heuristics and architecture policy extensions (Post-V1 E06).

## Normative requirements
- Analyzer runs after successful static checks and before evidence aggregation:
  - `WOLIO-ANALYZER-001` (error) rejects mutation requests attempting diagnostic
    suppression or verification bypass; repair classes never include `suppress`
    or `ignore` (DIAGNOSTIC_SPEC).
  - `WOLIO-ANALYZER-002` and `003` are descriptive aliases in the registry;
    emit `WOLIO-ERROR-001`, `WOLIO-DEPENDENCY-001` or `WOLIO-EFFECT-001` for
    language violations and never emit duplicate diagnostics (DIAGNOSTIC_SPEC).
  - `WOLIO-ANALYZER-004` (error) rejects exact integers (including folded unary
    minus) and strings listed in manifest `[analysis]`
    `forbidden_int_literals` / `forbidden_string_literals` when present in
    production implementation ASTs; tests, contract predicates and `intent`
    strings are excluded; policy is included in the snapshot hash
    (DIAGNOSTIC_SPEC).
  - `WOLIO-ANALYZER-005` (warning) emits one warning per `match` whose
    normalized arm bodies are structurally identical after bound-variable
    renaming; warnings do not fail `verify` or raise approval tier
    (DIAGNOSTIC_SPEC, APPROVAL_MATRIX_V1).
- `finalize` binds complete technical evidence to the published snapshot; any
  failed, missing, `not_run` or required `unknown` obligation blocks `apply` and
  `finalize` without recording successful evidence (LOCAL_PROTOCOL_V1,
  APPROVAL_MATRIX_V1, EVIDENCE_MODEL).
- `POST_REVIEW` records allow local build/test/finalize while `pending`, but
  block release artifacts and baseline acceptance until an owner records
  `accepted`. Agents cannot resolve their own review (`pending -> accepted` or
  `rejected`). `pending -> rejected` requires a fresh repair or revert plan
  against the current snapshot and never silently rolls back later work
  (LOCAL_PROTOCOL_V1, APPROVAL_MATRIX_V1).
- Repair is a new plan subject to all scope/tier/verification rules with a
  default budget of 3 attempts before reporting unresolved diagnostics
  (LOCAL_PROTOCOL_V1).

## Acceptance criteria
Positive and boundary:
- [ ] Unconfigured literals pass; literals in tests, contract predicates and `intent` are excluded from `WOLIO-ANALYZER-004`
- [ ] Structurally identical normalized match arms emit one `WOLIO-ANALYZER-005` warning while allowing `verify`, `apply` and `finalize` to succeed
- [ ] `AUTO` and `POST_REVIEW` plans finalize when all required gates pass; pending reviews are visible in local queries and allow local builds
- [ ] Owner `accepted` disposition clears release/baseline gating for a `POST_REVIEW` record

Rejection and failure:
- [ ] Configured `forbidden_int_literals` (including negative literals) and `forbidden_string_literals` in production implementations fail with `WOLIO-ANALYZER-004` and block `apply`
- [ ] Suppression/bypass requests fail with `WOLIO-ANALYZER-001`; `002`/`003` never duplicate `WOLIO-ERROR-001`/`DEPENDENCY-001`/`EFFECT-001`
- [ ] Agent self-review is rejected; `pending` or `rejected` review records block release eligibility and baseline acceptance
- [ ] Rejected review disposition requires a new plan against the current snapshot rather than silent rollback
- [ ] Failed, `not_run` or required `unknown` obligations block `finalize`; exhausting the 3-attempt repair budget stops and reports unresolved diagnostics

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
Evidence: `.execution/reports/T044.md` with AGENT repair/finalize/review and ANALYZER fixtures.

## Dependencies and open items
- Depends on: T043; blocks: T060, T070.
- Open: none.

