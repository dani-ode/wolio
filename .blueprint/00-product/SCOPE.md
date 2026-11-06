`.blueprint/00-product/SCOPE.md`
# Wolio Repository and Project Layout

## 1. Recommended user project

A generated project should be explicit but not needlessly rigid.

``` text
hello-world/
├── wolio.toml
├── wolio.lock
├── contracts/
│   └── project.contract.wol
├── decisions/
│   └── ADR-001.wol
├── src/
│   ├── main.wol
│   └── hello/
│       ├── hello.contract.wol
│       ├── hello.wol
│       └── hello.test.wol
├── tests/
│   └── integration.wol
└── .wolio/
    ├── graph/
    ├── semantic/
    ├── contracts/
    ├── symbols/
    ├── decisions/
    └── cache/
```

This structure is a project convention, not a hardcoded semantic
identity system.

## 2. Manifest

Example:

``` toml
[project]
name = "hello-world"
version = "0.1.0"

[language]
version = "0.1"

[entrypoint]
module = "main"

[verification]
contracts = "required"
effects = "required"
dependencies = "required"
tests = "required"
security = "required"

[agent]
structured_mutations = true
scope_required = true
human_approval_for_contract_change = true
```

## 3. File roles

V1 may support:

``` text
project
module
contract
entity
type
enum
operation
implementation
test
property
error
repository
service
endpoint
event
config
migration
```

Roles are metadata and verification boundaries.

Do not require a separate file for every declaration unless benchmarks
show it helps AI.

## 4. Naming

Names should be deterministic and machine-friendly.

Prefer:

``` text
CreatePayment
CreatePaymentError
CreatePaymentContract
CreatePaymentTest
```

Do not create arbitrary aliases for the same semantic concept.

## 5. Stable IDs

Declarations receive stable IDs.

Example:

``` text
operation CreatePayment
id = op_01J...
```

IDs must survive:

-   formatting;
-   ordinary renaming when explicitly tracked;
-   file moves;
-   import reordering.

## 6. Project contract

Example:

``` wol
project HelloWorld {
    verification:
        contracts
        effects
        dependencies
        tests
        security

    architecture:
        domain -> infrastructure = forbidden
}
```

## 7. Generated metadata

`.wolio/` contains derived information.

It may include:

``` text
semantic graph
symbol index
contract index
dependency graph
effect graph
test index
decision index
impact cache
```

It must be safe to delete and regenerate.

## 8. CLI

Initial commands:

``` bash
wolio init <name>
wolio check
wolio build
wolio test
wolio run
wolio fmt
wolio inspect <symbol>
wolio impact <symbol>
wolio graph
wolio verify
wolio audit
```

AI-specific commands:

``` bash
wolio agent inspect
wolio agent plan
wolio agent apply
wolio agent verify
wolio agent impact
```

## 9. Example workflow

``` bash
wolio init payment-service
cd payment-service

wolio check
wolio test
wolio build
```

For an AI task:

``` bash
wolio agent plan --intent "add idempotency to payment creation"
wolio agent impact
wolio agent apply
wolio agent verify
```

The agent should not need direct shell access to every file for normal
changes.

## 10. Package manager

Use:

``` text
wolio.toml
wolio.lock
```

The package manager must record:

-   exact versions;
-   integrity hashes;
-   platform constraints;
-   semantic capabilities;
-   security metadata.

Dependency installation must not silently grant arbitrary capabilities.
