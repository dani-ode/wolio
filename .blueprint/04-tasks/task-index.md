# Task index

Read DOCUMENT_AUTHORITY and BASELINE-V1 first. Task contracts are immutable design;
live task states belong to `.execution/task-status.md`.

| Stage | Task contracts | Entry dependency |
|---|---|---|
| M01 | T001-workspace-skeleton.md; T002-minimal-lexer.md; T003-minimal-parser-ast.md; T004-diagnostics-cli.md | owner baseline acceptance |
| M02 | EXECUTION_CONTRACTS.md: T020–T024 | M01 |
| M03 | EXECUTION_CONTRACTS.md: T030–T034 | M02 |
| M04 | EXECUTION_CONTRACTS.md: T040–T044 | M03 |
| M05 | EXECUTION_CONTRACTS.md: T050–T053 | M03 |
| M06 | EXECUTION_CONTRACTS.md: T060–T063 | M04 |
| M07 | EXECUTION_CONTRACTS.md: T070–T074 | M04 and M05 |

Only listed IDs are allocated; gaps reserve numbering, not hidden tasks. Future
extensions E01–E10 are design work packages in POST_V1_PLAN, not executable V1 tasks.
Reports use T001.md for implementation and PREP-0001.md for preparation. READY
means the contract exists, dependencies passed and the baseline is accepted.
