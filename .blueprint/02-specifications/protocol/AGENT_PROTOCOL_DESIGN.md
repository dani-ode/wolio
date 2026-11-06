# Agent Protocol Design V1

> LOCAL_PROTOCOL_V1.md resolves transport, authentication boundary, persistence,
> and wire schema for M04. Earlier OPEN items below are superseded.

**Status:** `Proposed`  
**Lifecycle:** `DISCOVER → UNDERSTAND → PROPOSE → AUTHORIZE → MUTATE → VERIFY → REPAIR → FINALIZE`.

Requests carry intent, allow/deny scope, constraints, capabilities, and a
request ID. The protocol exposes semantic entities, decisions, contracts,
impact, diagnostics, and verification status without requiring whole-repo
context. Mutations are structured (`create`, `modify`, `delete`, `rename`,
`move`, `add_test`, `modify_contract`, `add_dependency`, `add_effect`).

Finalization is impossible without scope validation and verification. Contract,
security-boundary, secret, unsafe, and scope-expansion changes require review.
Semantic diffs include affected contracts/tests/modules and risk.

`OPEN`: transport, authentication, persistence format, and exact wire schema.
