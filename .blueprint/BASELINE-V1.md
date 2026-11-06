# Wolio V1 — Freeze Baseline

**Status:** freeze candidate, 2026-11-06. Design revision: `1.2`.

Read `DOCUMENT_AUTHORITY.md` for the complete active-document register and
`00-product/PROJECT_COVERAGE.md` for coverage beyond the trial. Revision 1.2
clarifies interfaces, task contracts, historical authority and preparation status.

This document resolves the earlier V1 scope ambiguities. It takes precedence
over earlier aspirational examples and milestone ordering. Implementation is
not implied by design completion. The project owner records the actual freeze
in `.execution/baseline-acceptance.md` after reviewing this revision.

## Product decision

Wolio is a small contract-first programming language for AI-assisted development.
Its differentiator is the combination of behavioral contracts, nominal domain
types, explicit errors/effects, and actionable semantic feedback. Traits and
interfaces alone are not the comparison: benchmark against conventional
languages with their normal tests, linters, and validation libraries.

Readable, concise, predictable syntax is a requirement. The compiler cannot
establish whether human requirements are correct. Agent orchestration is a
consumer of compiler services, not a prerequisite for running the language.

## Delivery profiles

| Profile | Delivery | Included | Exit evidence |
|---|---|---|---|
| Frontend | M01 | init, lexer, parser, AST, JSON diagnostics | valid/invalid grammar fixtures |
| Executable core | M02 | resolution, types, interpreter, run, nominal types, Result/Option, exhaustive match | executable examples and negative type fixtures |
| Trial | M03 | pure contracts, tests, effect/dependency checks, inspect/impact, pilot benchmark | contract failures, three change cycles, raw pilot results |
| Agent | M04 | local structured mutation protocol, scope, approvals, atomic application | stale/rejected/accepted mutation fixtures |
| Native | M05 | typed IR, LLVM lowering, runtime parity | interpreter/native differential tests |
| Evaluation | M06 | expanded controlled benchmark | reproducible report, including negative findings |
| Toolchain | M07 | Git packages, formatter, LSP, distribution | offline locked build, format idempotence, editor diagnostics |

M03 is a useful and legitimate stopping point. M04 and M05 independently depend
on M03. M06 requires M04, not native execution; native results are a separate
benchmark stratum. M07 depends on M04 and M05. V1 ends at M07; stateful contracts,
shared-memory concurrency, formal proving, FFI, macros, inheritance, and arbitrary
generics are explicitly outside V1. Their extension boundaries are defined in
`02-specifications/EXTENSION_BOUNDARIES.md`, not promised implementations.

## Normative reading map

- Concrete syntax and executable semantics: `02-specifications/language/CORE_V1.md`.
- Behavioral obligations: `02-specifications/CONTRACT_DESIGN.md`.
- Identity, changes, and evidence: `02-specifications/SEMANTIC_MODEL.md`.
- Verification evidence: `02-specifications/verification/EVIDENCE_MODEL.md`.
- Local protocol: `02-specifications/protocol/LOCAL_PROTOCOL_V1.md`.
- Reproduction/version policy: `03-implementation/DELIVERY_POLICY.md`.
- Complete acceptance path: `06-project-management/DELIVERY_PLAN.md`.

Earlier `.wol` examples outside CORE_V1 are conceptual unless accepted by its
grammar. Unsupported syntax must produce a diagnostic, never silently degrade.
Earlier `OPEN` items are historical, not automatically resolved decisions. Only
an explicit normative rule resolves a question; a genuine uncovered semantic
decision blocks its affected task and requires an amendment.

## Freeze and amendment

Freeze fixes product behavior, feature boundaries, acceptance cases, and schema
versions. It does not freeze implementation internals or falsely pin a future
tool version. Build versions and benchmark model IDs are recorded at execution.
An amendment requires an ADR containing old/new rules, rationale, compatibility,
affected fixtures/tasks, and owner decision. No silent reinterpretation is allowed.
Specification defects remain reportable after freeze. A changed requirement must
change its acceptance cases before an implementation is marked complete.
