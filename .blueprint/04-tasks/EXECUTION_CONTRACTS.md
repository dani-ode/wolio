`.blueprint/04-tasks/EXECUTION_CONTRACTS.md`
# V1 task execution rules

Shared rules for every registered V1 task. Each task's complete contract lives in
its own file registered in [task-index.md](task-index.md); this file holds only
what all tasks share and never restates one task's deliverables. Read
DOCUMENT_AUTHORITY, BASELINE-V1, the task file and the authority it lists.
Actual readiness and results live only in `.execution/task-status.md`.

## Single source per fact

| Fact | Sole source |
|---|---|
| Language, protocol, package and evidence behavior | normative documents listed in `../DOCUMENT_AUTHORITY.md` |
| Acceptance family definitions | `../05-quality/ACCEPTANCE_MATRIX.md` |
| Milestone order and milestone gates | `../06-project-management/DELIVERY_PLAN.md` |
| One task's objective, scope, dependencies, acceptance and verification | that task's file |
| Task ID allocation and file location | `task-index.md` |
| Live status, blockers, decisions and evidence | repository `.execution/` |

Task files summarize normative rules for traceability only. On any difference
the cited normative document wins and the difference is a documentation defect:
stop the affected task, record the exact conflict and fix it (a semantic change
requires an ADR under BASELINE-V1). Mirrors of task facts — the task-index
dependency column, ACCEPTANCE_MATRIX owner column, DELIVERY_PLAN work units and
task-status rows — must match the task file; a mismatch blocks the task until
corrected in one documentation change.

## Task registry and change control

- One ID has exactly one file `Tnnn-<kebab-slug>.md`, one task-index row and one
  task-status row. The ID in the file heading equals the filename ID.
- IDs are allocated only by registering them in task-index. Unlisted numbers
  are reserve, not hidden tasks.
- Numbering blocks: M01 T001–T019, M02 T020–T029, M03 T030–T039, M04 T040–T049,
  M05 T050–T059, M06 T060–T069, M07 T070–T079. A new task takes the next unused
  ID in its milestone block; exhausting a block requires an amendment.
- IDs are never reused, renumbered or repurposed. A withdrawn task keeps its
  file and rows, marked with the decision that withdrew it.
- Adding, splitting, merging or withdrawing a task, or changing its dependencies
  or acceptance assignment, updates in the same change: the task file(s),
  task-index, task-status, affected DELIVERY_PLAN/ACCEPTANCE_MATRIX mirrors and
  CHANGELOG. After owner baseline acceptance this is an amendment.
- Every behavior has exactly one implementing task. Each task file lists its
  out-of-scope items with their owner task. When a later task needs an earlier
  task's analysis, it consumes that analysis instead of reimplementing it.
  Unclear ownership is a blocker to record, never a free choice.
- Priority: every registered task is `P0` because V1 completion requires all of
  them (BASELINE-V1). Execution order is determined only by dependencies.
- POST_V1_PLAN packages E01–E10 are design work, never V1 task IDs.

## Common gates

All tasks inherit the common checks in ACCEPTANCE_MATRIX. Every Rust work unit
runs `cargo fmt --all -- --check`,
`cargo clippy --workspace --all-targets -- -D warnings` and
`cargo test --workspace`; T001 additionally runs `cargo check --workspace`.
Store compiler conformance fixtures under `tests/conformance/<family>/`, executed
by host-side tests through the real frontend/runtime. Keep tests independent of
network except explicitly marked Git integration tests with controlled
repositories; never require production credentials in CI.

Bounded paths in task files are responsibility areas: choose the smallest useful
crate layout and record that choice before editing. Record actual test names and
commands in the execution report. Do not invent language semantics to fill an
engineering detail; an `OPEN` item in a task file must be resolved by its named
authority before the affected acceptance item can pass.

## Readiness and execution

READY means the task file exists, every `Depends on` task is DONE and the owner
baseline is accepted. When asked to continue, choose the first eligible task; do
not silently execute later tasks when the user assigns one ID. Prerequisites
outside the repository (LLVM build, model API access and cost, owner license
decision) that are missing make the task BLOCKED with a record under
`.execution/blockers/`; they never permit simulated evidence.

## Report contract

Use `.execution/reports/Tnnn.md` for implementation and `PREP-nnnn.md` for
preparation: task ID, baseline identity, changed paths, implemented behavior,
commands/exit results, fixture families, limitations and follow-up. DONE
requires every acceptance item of the task file. A blocked command is not passed.
