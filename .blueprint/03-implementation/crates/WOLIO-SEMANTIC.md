`.blueprint/03-implementation/crates/WOLIO-SEMANTIC.md`
# wolio-semantic

## Responsibility

Build the semantic model from AST + project configuration.

This crate is the core source of truth for compiler-facing meaning.

## Outputs

``` rust
pub struct SemanticRepository {
    pub modules: ModuleIndex,
    pub symbols: SymbolIndex,
    pub operations: OperationIndex,
    pub contracts: ContractIndex,
    pub types: TypeIndex,
    pub tests: TestIndex,
    pub relations: RelationGraph,
}
```

## Semantic IDs

Every semantic entity receives a stable ID.

Example:

``` text
fn_01J...
contract_01J...
type_01J...
module_01J...
```

The ID must not depend on:

-   line number
-   formatting
-   absolute filesystem path

## Relations

V1 relations:

``` text
requires
consumes
produces
transforms
depends_on
invalidates
affects
reads
writes
emits
```

## Core queries

``` rust
find_symbol(id)
find_operation(name_or_id)
find_dependents(id)
find_dependencies(id)
find_callers(id)
find_callees(id)
find_tests(id)
find_invariants(id)
```

## Invariants

-   one semantic ID maps to one entity in a repository version
-   unresolved references are represented explicitly
-   relation endpoints must exist
-   semantic graph is derived from source/configuration

## Tests

-   stable ID tests
-   symbol resolution fixtures
-   relation graph fixtures
-   cross-module references
-   rename/move stability

## Done when

A large project can be queried semantically without parsing raw source
for every query.
