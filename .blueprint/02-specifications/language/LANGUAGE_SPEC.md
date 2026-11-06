`.blueprint/02-specifications/language/LANGUAGE_SPEC.md`
# Wolio Language Specification --- V1 Baseline

> Revision 1.1: [CORE_V1.md](CORE_V1.md) is the normative grammar and semantics.
> Examples below retain conceptual long-term design, not accepted V1 syntax.
> Human readability and concise consistent syntax are requirements. Entities,
> provenance, secrets, concurrency, and architecture contracts are post-V1.

## 1. Design target

Wolio source may be verbose. Human readability is secondary to
explicitness and machine verification.

The core semantic unit is an **operation**, not an ordinary function.

``` wol
operation CalculateOrderSubtotal {
    consumes:
        ProductUnitPrice
        OrderQuantity

    requires:
        PriceContext = Checkout
        QuantityState = Validated
        CurrencyCompatibility = Same

    produces:
        OrderSubtotal

    effects:
        none
}
```

An implementation is attached to the operation.

## 2. Core declaration categories

V1 should support:

``` text
module
entity
type
enum
error
operation
implementation
contract
test
property
event
```

Do not add classes, inheritance, macros, reflection, or advanced
generics until the semantic core is stable.

## 3. Static typing

Primitive types:

``` text
Bool
Int
UInt
Float
Decimal
String
Bytes
Duration
Timestamp
```

Parameterized standard types:

``` text
Option<T>
Result<T, E>
List<T>
Map<K, V>
```

No implicit conversion between unrelated semantic types.

## 4. Semantic types

Representation type is not semantic identity.

Example:

``` wol
type ProductPrice = Money<IDR>
type TaxAmount = Money<IDR>
type AccountBalance = Money<IDR>
type PaymentAmount = Money<IDR>
```

All may use the same physical representation but must not be
interchangeable.

This prevents category errors such as assigning an account balance to a
product price.

## 5. Provenance

Values may carry compile-time semantic provenance.

Example:

``` wol
value email {
    type: Email
    source: HTTP.Request.Body
    trust: Untrusted
}
```

After validation:

``` wol
let verified_email = validate_email(email)
```

the returned value has a stronger semantic state.

The compiler should prevent APIs requiring validated input from
receiving untrusted input.

## 6. Entities

Use semantic entities rather than OOP classes as the primary domain
model.

Example:

``` wol
entity User {
    identity:
        id: UserId

    attributes:
        name: PersonName
        email: Email

    invariants:
        email.is_verified

    lifecycle:
        created_by: UserRegistration
        deleted_by: UserDeletion
}
```

## 7. Contracts

Contracts are versioned and protected.

``` wol
contract CreatePayment {
    input:
        request: PaymentRequest

    requires:
        request.amount > 0
        request.idempotency_key.valid

    ensures:
        result.payment.exists

    errors:
        InvalidAmount
        DuplicateRequest
        PaymentUnavailable

    effects:
        Payment.Write
        Ledger.Write
}
```

Implementation cannot silently weaken or change the contract.

A contract change produces a proposal and impact report.

## 8. Effects

Effects must be explicit.

Examples:

``` text
Database.Read
Database.Write
Network.Request
Filesystem.Read
Filesystem.Write
Task.Spawn
Task.Detach
Secret.Read
Clock.Read
```

A function with no declared effect cannot perform an effectful
operation.

## 9. Dependencies

Dependencies are explicit.

``` wol
operation CreatePayment {
    dependencies:
        PaymentRepository
        LedgerRepository
        Clock
        IdempotencyStore
}
```

No hidden global dependency is allowed.

## 10. Errors

Use typed errors.

``` wol
Result<Payment, CreatePaymentError>
```

No generic `throw` escape hatch in the core language.

All declared errors must be handled according to the project's policy.

## 11. Fallback semantics

Differentiate:

``` text
default
fallback
recovery
substitution
mock
placeholder
```

A default can be legal:

``` wol
default timeout = 30s
```

A synthetic substitute for a failed required operation is not legal
unless explicitly authorized.

Example:

``` text
UserNotFound → DefaultUser
```

must be rejected when fallback is forbidden.

## 12. Intent

Operations can declare intent.

``` wol
operation SaveUser {
    intent:
        PersistUser

    effects:
        Database.Write
}
```

The analyzer uses intent to detect effect substitution and workaround
behavior.

## 13. Secrets

Secrets are semantic values.

``` wol
secret StripeApiKey
```

A secret cannot be initialized from a source literal.

Legal:

``` wol
config.stripe.api_key
```

Illegal:

``` wol
let key: Secret<StripeApiKey> = "abc123"
```

## 14. No suppression

There is no general mechanism to suppress semantic verification.

An `unsafe` block must contain:

``` text
reason
owner
contract
tests
audit metadata
```

## 15. Tests

Project policy may require at least:

-   one normal-path test;
-   one failure/boundary test.

The compiler verifies the declared minimum. Analyzer checks whether
tests are meaningfully distinct.

Example:

``` wol
test create_payment_valid
test create_payment_duplicate_idempotency_key
```

## 16. Architecture contracts

Architecture is project policy, not hardcoded compiler doctrine.

Example:

``` wol
architecture {
    layer domain
    layer application
    layer infrastructure

    allow:
        domain -> domain
        application -> domain
        application -> infrastructure

    deny:
        domain -> infrastructure
}
```

## 17. Concurrency model

Concurrency is semantic.

Initial primitives:

``` text
task
parallel
transaction
channel
atomic
```

Later:

``` text
ownership
shared state
distributed ordering
idempotency
```

Database and distributed concurrency are first-class concerns.

Example:

``` wol
operation CreatePayment {
    idempotency:
        required

    key:
        request.idempotency_key

    guarantee:
        same_key -> same_payment
}
```

## 18. Syntax policy

Do not freeze every syntactic detail before semantic tests exist.

The first compiler milestone should prove:

-   parser stability;
-   semantic model stability;
-   deterministic diagnostics;
-   contract/type/effect checking.

Syntax may be refined while these foundations remain stable.
