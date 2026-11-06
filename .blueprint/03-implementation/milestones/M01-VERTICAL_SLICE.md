`.blueprint/03-implementation/milestones/M01-VERTICAL_SLICE.md`
# Milestone 1 --- Compiler Vertical Slice

> Revision 1.1: frontend only, using CORE_V1. Resolution/IDs move to M02.
> DELIVERY_PLAN controls acceptance over the historical scope below.

Do not begin by implementing every crate.

## Goal

Make this command work:

``` bash
wolio init hello
wolio check
```

with a real parser and semantic model.

## Scope

Implement:

1.  Cargo workspace
2.  lexer
3.  parser
4.  AST
5.  semantic IDs
6.  basic name resolution
7.  diagnostics
8.  CLI

## Explicitly postpone

-   LLVM
-   package registry
-   concurrency
-   LSP
-   formal verification
-   advanced analyzer
-   AI mutation application

## Acceptance

A valid hello-world project:

``` text
source
→ lex
→ parse
→ AST
→ semantic model
→ verify
→ JSON diagnostics
```

An invalid project must fail deterministically with a machine-readable
diagnostic.
