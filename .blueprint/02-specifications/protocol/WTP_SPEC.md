`.blueprint/02-specifications/protocol/WTP_SPEC.md`
# Wolio AI Agent Protocol

> LOCAL_PROTOCOL_V1.md is the normative local V1 binding. JSON below illustrates
> concepts, not additional wire formats. Finalization uses EVIDENCE_MODEL.md;
> unsupported extension analyses are not silently reported as passing.

## 1. Purpose

The Agent Protocol is the main AI-native layer.

An agent should not need to load an entire repository into context.

The system exposes:

-   project state;
-   relevant semantic entities;
-   decisions;
-   contracts;
-   allowed scope;
-   capabilities;
-   impact analysis;
-   verification status;
-   structured mutations.

## 2. Agent lifecycle

``` text
DISCOVER
  ↓
UNDERSTAND
  ↓
PROPOSE
  ↓
AUTHORIZE
  ↓
MUTATE
  ↓
VERIFY
  ↓
REPAIR
  ↓
FINALIZE
```

An agent must not jump directly from prompt to arbitrary file mutation.

## 3. Change request

Example:

``` json
{
  "request_id": "chg_01...",
  "intent": "Add passkey authentication",
  "scope": [
    "auth"
  ],
  "forbidden": [
    "payment",
    "billing"
  ],
  "constraints": [
    "password login remains available",
    "session expiry remains unchanged"
  ]
}
```

## 4. Change scope

Every AI task receives:

``` text
ALLOW
DENY
READ
WRITE
CREATE
DELETE
```

Example:

``` text
ALLOW:
    src/auth/**
    tests/auth/**

DENY:
    src/payment/**
    src/billing/**
```

An out-of-scope mutation is a hard failure.

## 5. Capabilities

Capabilities represent authority.

Examples:

``` text
Capability.DatabaseRead
Capability.DatabaseWrite
Capability.NetworkRequest
Capability.SecretRead
Capability.SchemaMigration
Capability.UnsafeFFI
```

An agent does not automatically receive all capabilities.

## 6. Structured mutation API

Preferred actions:

``` text
create_module
create_entity
create_type
create_operation
create_contract
modify_operation
modify_contract
add_dependency
add_effect
add_test
rename_symbol
move_symbol
request_capability
request_scope
```

The protocol should return semantic diffs, not only text diffs.

## 7. Semantic diff

Example:

``` text
ADDED operation CreatePayment
ADDED dependency IdempotencyStore
ADDED effect Database.Write
MODIFIED contract CreatePayment
ADDED test duplicate_request
```

The system should also report:

``` text
affected contracts: 12
affected tests: 48
affected modules: 4
risk: HIGH
```

## 8. Contract-change workflow

AI detects a missing requirement.

It must create:

``` text
CONTRACT_CHANGE_PROPOSAL
```

with:

``` text
current contract
proposed contract
reason
affected entities
affected tests
affected packages
risk
required approval
```

The implementation must not silently change the contract.

## 9. Repair protocol

Compiler errors must be machine-readable.

Example:

``` json
{
  "code": "WOLIO-EFFECT-004",
  "category": "EFFECT",
  "target": "fn_01...",
  "violation": "Database.Write",
  "declared_effects": [],
  "repairable": true,
  "repair_classes": [
    "remove_operation",
    "move_operation",
    "modify_contract",
    "introduce_dependency"
  ],
  "forbidden_repairs": [
    "suppress",
    "fallback",
    "workaround"
  ]
}
```

The AI should receive a repair space instead of guessing.

## 10. Workaround detection

The system should detect when an agent replaces an intended capability
with an unrelated mechanism.

Example:

``` text
Intent:
    PersistUser

Expected capability:
    Database.Write

Observed:
    HTTP.Request -> internal persistence endpoint
```

This is a semantic substitution and should be rejected or escalated.

## 11. Decision memory

Long-lived decisions become first-class artifacts.

Example:

``` wol
decision ADR_183 {
    idempotency {
        required = true
        storage = IdempotencyStore
    }

    reason:
        "Prevent duplicate payment creation."
}
```

The agent can query decisions without reconstructing historical
conversations.

## 12. Repository query interface

The protocol should support queries such as:

``` text
find_symbol
find_contract
find_dependents
find_dependencies
find_affected_tests
find_decisions
find_capabilities
find_invariants
find_operations_by_intent
impact
explain_symbol
explain_violation
```

## 13. Verification gate

A change cannot be finalized until:

``` text
scope = valid
typecheck = pass
contracts = pass
effects = pass
dependencies = pass
security = pass
tests = pass
regression = pass
```

For high-risk projects:

``` text
semantic_analysis = pass
concurrency_analysis = pass
architecture_analysis = pass
```

## 14. Human authority

Human approval is mandatory for:

-   contract changes;
-   security boundary changes;
-   secret/capability escalation;
-   architecture rule changes;
-   schema migration;
-   unsafe operations.

The AI can propose; it cannot self-authorize these changes.
