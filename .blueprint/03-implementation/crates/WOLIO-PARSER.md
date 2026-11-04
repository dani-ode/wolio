# wolio-parser

## Responsibility

Parse tokens into a lossless-enough abstract syntax tree.

## Inputs

`TokenStream`

## Outputs

`wolio_ast::Ast`

## V1 grammar surface

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

Expressions initially include:

-   literals
-   identifiers
-   calls
-   field access
-   unary/binary operators
-   `if`
-   `match`
-   block
-   `return`
-   explicit effectful operations

## Error recovery

The parser may recover enough to produce multiple diagnostics, but
recovery must never create a semantically valid AST node that was not
represented by source.

## Public API

``` rust
pub fn parse(source: &str) -> ParseResult;
```

## Invariants

-   every node has a span
-   every declaration has a stable syntactic identity candidate
-   parser never resolves names
-   parser never checks contracts
-   parser never checks dependencies

## Tests

-   golden AST fixtures
-   malformed syntax
-   nested expressions
-   exhaustive declaration forms
-   recovery behavior

## Done when

All V1 language examples parse into deterministic ASTs and malformed
programs produce structured diagnostics.
