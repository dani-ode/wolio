# wolio-package

## Responsibility

Resolve, lock, install, and verify immutable packages.

## Manifest

`wolio.toml`

Contains:

-   package identity
-   compiler version
-   dependencies
-   architecture policy
-   capability policy
-   source roots

## Lockfile

`wolio.lock`

Must include:

``` text
package
version
source_hash
dependency_hash
compiler_version
contract_hash
```

## Rules

-   immutable package identity
-   content-addressed source
-   deterministic dependency resolution
-   compiler version mismatch requires explicit migration
-   lockfile drift is a hard verification failure in locked builds

## Commands

``` text
wolio add
wolio remove
wolio install
wolio update
```

## Tests

-   lockfile generation
-   hash mismatch
-   compiler mismatch
-   dependency conflict
-   reproducible resolution
