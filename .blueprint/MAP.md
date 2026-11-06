# Blueprint Map — Start Here

**Current status:** `REVISION 1.1 FREEZE CANDIDATE / NO IMPLEMENTATION YET`

Start with [BASELINE-V1.md](BASELINE-V1.md), then
[DELIVERY_PLAN.md](06-project-management/DELIVERY_PLAN.md). These resolve the
earlier roadmap's scope and ordering. Actual status lives in `.execution/`.

Wolio is an AI-native language and construction system that turns intent into
bounded, structured, verifiable code changes.

## Execution chain

`Intent → Contract → Scoped change → Semantic analysis → Implementation → Verification`

## Authority ladder

1. [`blueprint.md`](blueprint.md) — vision and principles
2. `01-architecture/` — system boundaries
3. `02-specifications/` — language/protocol behavior
4. `03-implementation/` — crates and milestones
5. `04-tasks/` — executable work
6. `05-quality/` — quality gates
7. `07-ai-context/` — agent rules

## Delivery order

| ID | Milestone | Depends on | Status | Entry point |
|---|---|---|---|---|
| M00 | Preparation | — | `REVIEW` | `BASELINE-V1.md` |
| M01 | Vertical slice | M00 acceptance | `PLANNED` | `03-implementation/milestones/M01-VERTICAL_SLICE.md` |
| M02 | Semantic core | M01 | `PLANNED` | `03-implementation/milestones/M02-SEMANTIC_CORE.md` |
| M03 | Verification | M02 | `PLANNED` | `03-implementation/milestones/M03-VERIFICATION.md` |
| M04 | Agent protocol | M03 | `PLANNED` | `03-implementation/milestones/M04-AGENT_PROTOCOL.md` |
| M05 | Native backend | M03 | `PLANNED` | `03-implementation/milestones/M05-NATIVE.md` |
| M06 | Benchmark | M04 | `PLANNED` | `03-implementation/milestones/M06-BENCHMARK.md` |
| M07 | Toolchain | M04, M05 | `PLANNED` | `06-project-management/DELIVERY_PLAN.md` |

## Read by change type

| Change | Start here |
|---|---|
| Lexer/parser/AST | `02-specifications/language/LANGUAGE_SPEC.md` + relevant crate file |
| Contracts/effects/dependencies | `blueprint.md` + `01-architecture/SYSTEM_ARCHITECTURE.md` |
| Verification/diagnostics | `02-specifications/verification/` + `05-quality/DEFINITION_OF_DONE.md` |
| Agent mutations/scopes | `02-specifications/protocol/WTP_SPEC.md` + `07-ai-context/ENGINEERING_RULES.md` |
| Planning | `06-project-management/ROADMAP.md` + `04-tasks/task-index.md` |

## Non-negotiables

No silent behavior, no arbitrary AI edits by default, no semantic suppression,
and no contract change without impact analysis and explicit approval.
