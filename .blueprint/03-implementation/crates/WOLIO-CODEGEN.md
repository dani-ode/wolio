`.blueprint/03-implementation/crates/WOLIO-CODEGEN.md`
# wolio-codegen

## Responsibility

Lower verified Wolio IR to LLVM IR and native artifacts.

## Backend strategy

Do not implement a CPU backend.

Use:

``` text
Wolio IR
→ LLVM IR
→ LLVM
→ native executable
```

## Requirements

-   target triple support
-   debug/source mapping
-   deterministic code generation where practical
-   explicit runtime calls
-   no codegen for unverified IR

## Safety invariant

`wolio-codegen` must accept only verified IR.

A caller must not be able to invoke code generation on an unverified
semantic model.

## Tests

-   hello world
-   arithmetic
-   Result/Option lowering
-   function calls
-   effectful runtime calls
-   target compilation
