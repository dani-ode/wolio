# Task Index

Tasks are executable units. Choose the first `READY` task whose dependencies
are complete. Each task links authority, scope, acceptance criteria, and
verification evidence.

## M01 — Vertical slice

| ID | Task | Status | Depends on |
|---|---|---|---|
| T001 | Establish Rust workspace and crate skeleton | `READY` | — |
| T002 | Implement lexer for minimal source slice | `READY` | T001 |
| T003 | Implement parser and AST for minimal source slice | `READY` | T002 |
| T004 | Add deterministic diagnostics and CLI check | `READY` | T003 |

Task files follow `TASK-<id>-<slug>.md`. Copy `TASK_TEMPLATE.md` when adding one.
