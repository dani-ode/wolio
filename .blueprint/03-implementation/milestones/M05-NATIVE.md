# Milestone 5 --- Native Execution

> Requires M03 independently of M04. DELIVERY_POLICY requires interpreter/native
> parity across all conformance fixtures, not only hello world.

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
