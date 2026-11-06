`.blueprint/03-implementation/crates/WOLIO-AGENT.md`
# wolio-agent

## Responsibility

Provide the structured interface through which AI agents understand and
modify a Wolio repository.

This is the most important AI-native crate.

## Agent lifecycle

``` text
DISCOVER
→ UNDERSTAND
→ PROPOSE
→ AUTHORIZE
→ MUTATE
→ VERIFY
→ REPAIR
→ FINALIZE
```

## Read operations

``` text
inspect_symbol
inspect_operation
find_contract
find_dependencies
find_dependents
find_callers
find_callees
find_tests
find_invariants
find_decisions
impact
explain_violation
```

## Structured mutation operations

``` text
create_symbol
modify_symbol
modify_contract
add_test
add_dependency
declare_effect
rename_symbol
move_symbol
request_capability
request_scope
```

The agent must not need arbitrary text-file editing for semantic
mutations.

## Change scope

Every mutation request carries:

``` text
request_id
agent_id
scope
target semantic IDs
allowed operations
forbidden areas
requested capabilities
reason
```

## Authorization

The agent must never infer authority from its intelligence.

Authority is:

``` text
authorized scope
+ explicit capability
+ known contract
+ known dependencies
+ known invariants
```

## Contract changes

Require a proposal and human/project-policy authorization.

## Apply invariant

Every mutation must produce a new semantic state that passes
verification before finalization.

## Tests

-   inspect responses
-   invalid scope
-   unauthorized capability
-   valid mutation
-   contract-change rejection
-   rollback
-   deterministic mutation application
