# wolio-ir

## Responsibility

Define a stable intermediate representation between Wolio semantics and
backend code generation.

## Design goal

The IR should represent verified Wolio meaning without exposing
LLVM-specific details.

## Initial blocks

``` text
Module
Function
Block
Value
Instruction
Terminator
Type
Call
Branch
Return
```

## Requirements

-   deterministic serialization
-   stable IDs for debugging
-   source semantic ID mapping
-   explicit effect/capability metadata
-   no unresolved symbols after lowering

## Pipeline

``` text
SemanticRepository
    ↓
VerifiedModel
    ↓
Wolio IR
    ↓
LLVM IR
```

## Tests

-   lowering golden tests
-   invalid IR rejection
-   deterministic serialization
-   source mapping
