`.blueprint/01-architecture/SECURITY_ARCHITECTURE.md`
# Security and Authority Model

## Principle

AI capability does not imply authority.

The agent receives:

``` text
scope
capability
contract
dependencies
invariants
```

before mutation.

## Capability examples

``` text
Source.Read
Source.Write
Contract.Modify
Dependency.Add
Effect.Declare
Schema.Migrate
Network.Access
Secret.Read
Unsafe.Execute
```

Capabilities should be narrow.

## Forbidden defaults

No generic:

``` text
ignore
disable
lint-disable
no-check
unsafe-ignore
```

An unsafe operation, if eventually supported, must require:

-   explicit annotation
-   reason
-   owner
-   contract
-   tests
-   audit record
-   capability

## Secret rule

Secrets are semantic values. Literal secrets in source are forbidden.

## Mutation rule

No mutation outside authorized scope.

## Verification rule

No agent operation may finalize without verification.
