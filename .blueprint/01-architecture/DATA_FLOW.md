# Source of Truth and Generated State

## Source of truth

The authoritative project state is:

``` text
*.wol
wolio.toml
wolio.lock
contracts/
decisions/
tests/
```

## Derived state

`.wolio/` is rebuildable:

``` text
.wolio/
├── ast/
├── contracts/
├── graph/
├── symbols/
├── tests/
├── dependencies/
└── history/
```

It exists to accelerate AI/tooling queries.

Deleting `.wolio/` must not delete project meaning.

## Rebuild invariant

``` bash
rm -rf .wolio
wolio check
```

must recreate equivalent semantic metadata.

## History

Agent mutation history may be stored as derived audit information, but
source-of-truth decisions must remain in explicit project artifacts such
as contracts and decisions.
