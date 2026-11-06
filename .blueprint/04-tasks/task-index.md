# Task Index

Revision 1.1 authoritative work breakdown: `06-project-management/DELIVERY_PLAN.md`.
Actual task states live in `.execution/task-status.md`. The ranges below are
historical; M06 now depends on M04 only and M07 completes the toolchain.

Tasks are executable units. Choose the first `READY` task whose dependencies
are complete. Each task links authority, scope, acceptance criteria, and
verification evidence.

## M00 — Preparation

The M00 session status and deliverables are tracked in
`06-project-management/M00-PREPARATION.md`. M01 tasks cannot start until the
M00 approval record is completed.

## M01 — Vertical slice

| ID | Task | Status | Depends on |
|---|---|---|---|
| T001 | Establish Rust workspace and crate skeleton | `BLOCKED` | M00 approval |
| T002 | Implement lexer for minimal source slice | `BLOCKED` | T001 |
| T003 | Implement parser and AST for minimal source slice | `BLOCKED` | T002 |
| T004 | Add deterministic diagnostics and CLI check | `BLOCKED` | T003 |

Task files follow `T<id>-<slug>.md`; execution reports use `TASK-<id>.md`.
Copy `TASK_TEMPLATE.md` when expanding a work unit.

## M02–M06 — Planned execution backlog

The following phase tasks are deliberately indexed before implementation. Their
status is `PLANNED` until the preceding milestone has passed its acceptance
gate. This makes the complete delivery path visible without claiming work has
been done.

| Phase | Task range | Scope | Status | Entry gate |
|---|---|---|---|---|
| M02 | T020–T029 | semantic model, symbols, types, Result/Option, resolution | `PLANNED` | M01 accepted |
| M03 | T030–T039 | verification, contracts, effects, dependencies, regression fixtures | `PLANNED` | M02 accepted |
| M04 | T040–T049 | agent protocol, repository graph, structured mutations, repair loop | `PLANNED` | M03 accepted |
| M05 | T050–T059 | Wolio IR, lowering, LLVM/native build, runtime | `PLANNED` | M03 accepted |
| M06 | T060–T069 | benchmark runner, baselines, evaluation, final report | `PLANNED` | M04 and M05 accepted |

Each range is expanded into individual task files immediately before that
phase starts. A phase task may be marked `READY` only when its authority
documents, scope, acceptance criteria, and verification command are present.

## Status protocol

- `PLANNED`: visible in the roadmap, not executable yet.
- `READY`: dependencies and task contract are complete.
- `IN_PROGRESS`: one agent session is executing the bounded task.
- `BLOCKED`: a dependency or human decision prevents execution.
- `DONE`: acceptance criteria are met and implementation evidence is recorded
  in `.execution/`.
