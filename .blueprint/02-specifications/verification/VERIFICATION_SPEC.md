# Wolio Verification, Security, and Anti-Workaround Blueprint

> Revision 1.1: EVIDENCE_MODEL.md defines enforceable V1 guarantees and statuses.
> Security/provenance/concurrency examples below describe extensions, not V1
> capabilities. Heuristics do not establish language illegality or business truth.

## 1. Verification layers

Verification must be layered.

### Layer 1 --- Syntax

Can the source be parsed?

### Layer 2 --- Type

Are values and operations type-correct?

### Layer 3 --- Contract

Does implementation satisfy preconditions, postconditions, errors, and
invariants?

### Layer 4 --- Effects

Are all effects declared and authorized?

### Layer 5 --- Dependencies

Are all dependencies explicit and directionally valid?

### Layer 6 --- Scope

Did the agent modify only authorized areas?

### Layer 7 --- Security

Are capability, authorization, secret, input-trust, and data-exposure
constraints satisfied?

### Layer 8 --- Regression

Do existing tests and invariants remain valid?

### Layer 9 --- Semantic analysis

Does the implementation appear to bypass intent or introduce suspicious
workarounds?

## 2. Important limitation

Contracts and tests do not prove that requirements themselves are
correct.

Therefore the system must preserve a distinction between:

``` text
intent
contract
implementation
test
```

A future formal verification layer should not pretend to solve incorrect
requirements.

## 3. Security model

Security concepts should be semantic:

``` text
Principal
Capability
Secret
TrustedInput
UntrustedInput
Authorization
DataExposure
```

Example:

``` wol
endpoint payment.get {
    requires:
        caller.can("payment.read")

    data:
        exposes Payment.public_fields

    forbids:
        Payment.internal_fields
}
```

## 4. Secret protection

Rules:

1.  secrets cannot be source literals;
2.  secrets cannot be printed;
3.  secrets cannot be serialized without explicit capability;
4.  secret values cannot silently cross trust boundaries;
5.  secret reads are effects;
6.  secret access is auditable.

## 5. Input provenance

External input starts as untrusted.

``` text
HTTP.Body
    ↓
Untrusted
    ↓
Validation
    ↓
Validated<Email>
```

An API requiring validated input must reject raw external data.

## 6. Fallback policy

Allowed:

``` text
default timeout = 30s
```

Potentially allowed:

``` text
recovery retry
```

Forbidden by default:

``` text
UserNotFound → DefaultUser
DatabaseError → hardcoded record
API failure → fake success
```

Every fallback must be explicitly classified.

## 7. Workaround policy

Detect:

-   effect substitution;
-   capability substitution;
-   hidden persistence;
-   temporary-file persistence;
-   global singleton bypass;
-   network-based bypass;
-   hardcoded business state;
-   mock/placeholder leakage into production.

## 8. Unsafe

`unsafe` is an audited capability.

Required metadata:

``` text
reason
owner
contract
tests
risk
```

No generic "ignore compiler" mechanism exists.

## 9. Concurrency verification

The concurrency analyzer should eventually detect:

``` text
race condition
deadlock
lost update
double execution
TOCTOU
inconsistent state
```

Distributed operations must support idempotency when required.

Example:

``` wol
operation CreatePayment {
    idempotency:
        required

    guarantee:
        same_key -> same_payment
}
```

## 10. Security and verification output

Diagnostics must contain:

``` text
code
severity
category
semantic target
source location
explanation
expected state
observed state
repair classes
forbidden repairs
impact
```

This is both a developer interface and an AI repair protocol.

## 11. Verification policy file

Projects should be able to define strictness:

``` toml
[verification]
type_check = "required"
contracts = "required"
effects = "required"
dependencies = "required"
tests = "required"
security = "required"
semantic_analysis = "required"
concurrency_analysis = "required"
```

The language remains generic; the project chooses its verification
level.
