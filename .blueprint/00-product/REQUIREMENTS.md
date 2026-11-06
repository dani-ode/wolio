# Wolio Requirements — M00 Baseline

> Revision 1.1: requirements map to M01 (FR-001/004), M02 (FR-002),
> M03 (FR-003/006 query portion), M04 (FR-005/006 mutation portion/007).
> Runnable interpreter, nominal types, exhaustive matching, guarded contracts,
> and independent pilot evidence are mandatory M03 requirements. Native parity
> is M05; packaging/editor/distribution is M07. DELIVERY_POLICY.md resolves
> version/platform/CI selection; DELIVERY_PLAN.md defines measurable acceptance.

**Status:** `Proposed`

## Functional requirements

| ID | Requirement | Acceptance evidence |
|---|---|---|
| FR-001 | Parse valid and invalid V1 source deterministically | parser tests/snapshots |
| FR-002 | Assign stable semantic IDs independent of paths | ID fixture |
| FR-003 | Represent and verify contracts, errors, effects, dependencies | semantic fixtures |
| FR-004 | Emit versioned machine-readable diagnostics | diagnostic fixtures |
| FR-005 | Reject out-of-scope or unauthorized mutations | protocol tests |
| FR-006 | Produce semantic diff, impact, provenance, and verification result | agent fixtures |
| FR-007 | Preserve decisions and require explicit approval for high-risk changes | audit records |

## Non-functional requirements

Determinism, reproducibility, explicit failure, least authority, stable schemas,
regression resistance, inspectability, and evidence-based evolution are required.

## Constraints and OPEN items

Rust stable and Cargo are the planned implementation baseline. Exact pinned
versions, CI providers, and performance targets are `OPEN` until M00 approval.
