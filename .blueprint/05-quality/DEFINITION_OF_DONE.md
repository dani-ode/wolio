# Wolio Definition of Done

## A. Language

-   [ ] Grammar is versioned.
-   [ ] Parser is deterministic.
-   [ ] AST is stable enough for tooling.
-   [ ] Semantic IDs exist.
-   [ ] Type checking works.
-   [ ] Result/Option works.
-   [ ] Exhaustive matching works.
-   [ ] Typed errors work.

## B. Contracts

-   [ ] Preconditions work.
-   [ ] Postconditions work.
-   [ ] Errors are declared.
-   [ ] Effects are declared.
-   [ ] Dependencies are declared.
-   [ ] Invariants are represented.
-   [ ] Contract changes are detected.
-   [ ] Contract impact is calculated.

## C. AI Construction System

-   [ ] Agent can inspect project state.
-   [ ] Agent can request a change scope.
-   [ ] Agent can create a plan.
-   [ ] Agent can inspect impact.
-   [ ] Agent can perform structured mutations.
-   [ ] Agent cannot mutate outside scope.
-   [ ] Agent receives structured diagnostics.
-   [ ] Agent can perform bounded repair loops.
-   [ ] Agent cannot suppress semantic violations.

## D. Anti-workaround

-   [ ] Illegal fallback is rejected.
-   [ ] Secret literals are rejected.
-   [ ] Effect substitution is detected.
-   [ ] Capability substitution is detected.
-   [ ] Hidden persistence is detected.
-   [ ] Hardcoded business-value patterns are analyzed.

## E. Verification

-   [ ] Type verification.
-   [ ] Contract verification.
-   [ ] Effect verification.
-   [ ] Dependency verification.
-   [ ] Scope verification.
-   [ ] Security verification.
-   [ ] Regression testing.
-   [ ] Semantic analysis.

## F. Concurrency

-   [ ] Task model exists.
-   [ ] Structured concurrency exists.
-   [ ] Transactions exist.
-   [ ] Idempotency exists.
-   [ ] Concurrency fixtures exist.
-   [ ] Race/lost-update cases are tested.

## G. Compiler

-   [ ] Wolio IR exists.
-   [ ] LLVM backend works.
-   [ ] Native binaries build.
-   [ ] Build is reproducible enough for the target platform.

## H. Tooling

-   [ ] CLI.
-   [ ] Formatter.
-   [ ] Test runner.
-   [ ] LSP.
-   [ ] Package manager.
-   [ ] Lockfile.
-   [ ] Agent API.

## I. Evidence

Wolio must have benchmark evidence showing whether it improves AI
coding.

Minimum report:

``` text
Baseline:
    GPT + Go
    GPT + Rust
    GPT + TypeScript

Wolio:
    GPT + Wolio

Metrics:
    task success
    semantic bugs
    regressions
    repair iterations
    compiler loops
    tokens
    time
    security defects
    concurrency defects
```

## J. Final acceptance test

A realistic project must survive at least three sequential AI change
cycles:

``` text
Cycle 1:
    implement feature

Cycle 2:
    modify existing feature

Cycle 3:
    add unrelated feature
```

The final cycle must demonstrate that the AI can discover and obey the
previous architecture, contracts, decisions, and invariants without
requiring the entire project in context.

That is the real completion criterion.
