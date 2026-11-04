# wolio-effects

## Responsibility

Define and verify explicit side effects.

## V1 effect vocabulary

``` text
Database.Read
Database.Write
Network.Read
Network.Write
File.Read
File.Write
Clock.Read
Random.Read
Environment.Read
Event.Emit
Task.Spawn
Task.Detach
```

The list is extensible.

## Effect model

``` rust
pub struct EffectSet {
    pub effects: BTreeSet<Effect>,
}
```

## Rules

1.  An operation may only perform declared effects.
2.  Calling an operation requires compatibility with the caller's effect
    set.
3.  Pure code is `effects: none`.
4.  Time/random/environment access is explicit.
5.  Detached tasks require explicit capability/effect.

## Diagnostic

``` text
WOLIO-EFFECT-001
Undeclared effect
```

Diagnostic payload must identify:

-   operation
-   observed effect
-   declared effects
-   source span
-   repair classes

## Tests

-   pure operation rejecting database access
-   clock access
-   nested effect propagation
-   effect widening
-   undeclared effect diagnostics
