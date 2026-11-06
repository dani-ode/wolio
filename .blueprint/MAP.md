# Blueprint map

Revision 1.2 freeze candidate; no compiler implementation yet.

1. [Document authority](DOCUMENT_AUTHORITY.md)
2. [V1 baseline](BASELINE-V1.md)
3. [Whole-project coverage](00-product/PROJECT_COVERAGE.md)
4. [Delivery plan](06-project-management/DELIVERY_PLAN.md)
5. [Task index](04-tasks/task-index.md)
6. [Acceptance matrix](05-quality/ACCEPTANCE_MATRIX.md)
7. [Post-V1 plan](06-project-management/POST_V1_PLAN.md)
8. [Ubuntu and agent quickstart](07-ai-context/GETTING_STARTED.md)

| Change | Normative starting point |
|---|---|
| Lexer/parser/types/runtime | language/CORE_V1 + INTERFACE_CONTRACTS |
| Contracts/errors/effects | CONTRACT_DESIGN + CORE_V1 |
| Graph/identity/impact | SEMANTIC_MODEL + INTERFACE_CONTRACTS |
| Diagnostics/evidence | verification/DIAGNOSTIC_SPEC + EVIDENCE_MODEL |
| Agent mutations | protocol/LOCAL_PROTOCOL_V1 |
| Packages | package-manager/PACKAGE_MANAGER_SPEC + INTERFACE_CONTRACTS |
| Formatter/LSP/native | DELIVERY_POLICY + relevant acceptance families |

Specifications in the table are under `02-specifications/`; DELIVERY_POLICY is
under `03-implementation/`. Read only the task-relevant context after the baseline.
Actual progress: repository `.execution/current-state.md` and `task-status.md`.
