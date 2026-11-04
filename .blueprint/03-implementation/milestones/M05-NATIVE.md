# Milestone 5 --- Native Execution

## Goal

Execute verified Wolio programs natively.

Pipeline:

``` text
verified semantic model
→ Wolio IR
→ LLVM IR
→ native executable
```

## Acceptance

``` bash
wolio build
wolio run
```

must execute the hello-world example.

No backend work should leak into semantic crates.
