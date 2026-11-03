# wolio-contracts

## Responsibility

Represent and validate contracts.

## Contract model

A contract may declare:

``` text
requires
ensures
invariants
consumes
produces
dependencies
effects
idempotency
architecture constraints
```

Example:

``` text
contract withdraw {
    requires amount > 0
    ensures balance >= 0
}
```

## Core types

``` rust
pub struct Contract {
    pub id: ContractId,
    pub target: SemanticId,
    pub requires: Vec<Predicate>,
    pub ensures: Vec<Predicate>,
    pub invariants: Vec<Predicate>,
    pub dependencies: Vec<SemanticId>,
    pub effects: EffectSet,
}
```

## Validation

The contract crate validates structure and expression typing. It must
not pretend to prove arbitrary business logic.

## Important limitation

A contract-correct implementation may still implement the wrong business
algorithm. Such cases belong to tests, properties, analyzer checks, or
external/domain verification.

## Contract changes

Contract modification is a first-class semantic change and must expose:

-   current contract
-   proposed contract
-   reason
-   affected entities
-   affected tests
-   risk

## Tests

-   requires/ensures parsing
-   predicate typing
-   contract compatibility
-   contract diff
-   invariant propagation
