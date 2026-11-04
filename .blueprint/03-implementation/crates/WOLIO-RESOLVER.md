# wolio-resolver

## Responsibility

Resolve names, modules, imports, dependencies, and package references.

## Inputs

-   semantic AST
-   project manifest
-   lockfile
-   package registry metadata

## Outputs

``` rust
pub struct Resolution {
    pub symbols: SymbolResolution,
    pub dependencies: DependencyGraph,
}
```

## Explicit dependency rule

An implementation cannot silently introduce a dependency.

Example:

``` text
hello()
```

must not gain `Database.Write` merely because the implementation calls a
database service.

## Architecture

Project architecture rules are supplied by configuration:

``` text
layer domain
layer application
layer infrastructure

allow:
    application -> domain
    application -> infrastructure

deny:
    domain -> infrastructure
```

The resolver reports graph edges; policy verification is performed
downstream.

## Tests

-   unresolved symbol
-   ambiguous symbol
-   missing dependency
-   dependency direction
-   package version resolution
-   lockfile mismatch
