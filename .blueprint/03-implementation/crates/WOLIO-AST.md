`.blueprint/03-implementation/crates/WOLIO-AST.md`
# wolio-ast

## Responsibility

Define the canonical syntax representation shared by compiler and
tooling.

## Core model

``` rust
pub struct Ast {
    pub modules: Vec<Module>,
}

pub struct Module {
    pub id: SyntaxId,
    pub name: Name,
    pub declarations: Vec<Declaration>,
    pub span: Span,
}
```

Declaration variants:

``` text
Module
Entity
Type
Enum
Error
Operation
Implementation
Contract
Test
Property
Event
```

## Important design rule

AST identity is not semantic identity.

Semantic IDs are assigned by semantic analysis and remain stable across
ordinary formatting and source relocation.

## Node requirements

Every node must contain:

-   source span
-   syntactic kind
-   child references
-   optional documentation metadata

Do not put compiler state into AST nodes.

## Tests

-   construction invariants
-   serialization snapshot
-   visitor traversal
-   structural equality
-   source span propagation

## Non-goals

-   symbol table
-   type inference
-   contract evaluation
-   dependency graph
