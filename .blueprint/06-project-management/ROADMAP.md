# Wolio Implementation Roadmap

> Revision 1.1: DELIVERY_PLAN.md replaces the phase sequence below with the
> authoritative M01–M07 delivery path. This older roadmap remains design rationale.
> In particular pilot evidence is collected at M03, before native/protocol work.

## 0. Rule before implementation

Do not start by implementing the full language.

First prove that the architecture can be implemented incrementally and
that the AI-specific mechanisms actually solve measurable problems.

## Phase 0 --- Preparation

Deliver:

-   product definition;
-   language principles;
-   semantic model;
-   error taxonomy;
-   verification taxonomy;
-   benchmark specification;
-   repository structure;
-   ADR set;
-   contribution rules;
-   compatibility/versioning policy.

Exit condition:

> The team can explain what Wolio refuses to solve in V1.

## Phase 1 --- Compiler skeleton

Implement:

``` text
Rust workspace
lexer
parser
AST
diagnostic framework
CLI
```

First programs:

``` text
empty module
hello world
invalid syntax
```

Exit:

-   deterministic parsing;
-   structured diagnostics;
-   snapshot tests.

## Phase 2 --- Semantic core

Implement:

``` text
module
type
entity
operation
implementation
error
Result
Option
```

Add:

-   name resolution;
-   semantic IDs;
-   type checking.

Exit:

-   valid programs compile to semantic AST;
-   invalid programs fail deterministically.

## Phase 3 --- Contracts

Implement:

``` text
requires
ensures
errors
invariants
```

Add contract compatibility checking.

Contract modifications become explicit changes.

Exit:

-   implementation cannot violate declared contract;
-   contract changes produce impact reports.

## Phase 4 --- Effects and dependencies

Implement:

``` text
effects
dependencies
capabilities
```

Reject:

-   undeclared effect;
-   hidden dependency;
-   unauthorized capability.

Exit:

> A function's meaningful external behavior can be determined from its
> semantic model.

## Phase 5 --- Semantic/domain types

Implement:

``` text
type ProductPrice = ...
type AccountBalance = ...
```

Add provenance.

Exit:

-   semantically incompatible values cannot be substituted merely
    because their representations match.

## Phase 6 --- Tests and verification

Implement:

``` text
test declarations
test discovery
test runner
verification gate
```

Minimum project policy:

-   success path;
-   failure/boundary path.

Exit:

``` bash
wolio verify
```

must produce a machine-readable verification result.

## Phase 7 --- AI Agent Protocol

Implement:

``` text
inspect
plan
impact
apply
verify
```

Structured mutations:

``` text
create_operation
modify_operation
modify_contract
add_test
add_dependency
add_effect
rename_symbol
```

Exit:

> An AI can implement a feature without unrestricted text editing.

## Phase 8 --- Scope and capability security

Implement:

``` text
change scope
read/write permissions
capability requests
human approval
```

Exit:

> An AI cannot modify files outside the authorized semantic scope.

## Phase 9 --- Anti-workaround system

Implement analyzer detection for:

``` text
fallback substitution
effect substitution
capability bypass
hardcoded business values
secret literals
hidden persistence
```

Exit:

> Known workaround patterns are reliably rejected or escalated.

## Phase 10 --- Repository semantic graph

Implement:

``` text
symbol graph
contract graph
effect graph
dependency graph
impact graph
decision graph
```

Exit:

``` bash
wolio impact <symbol>
```

returns accurate affected artifacts.

## Phase 11 --- Native backend

Implement:

``` text
Wolio IR
LLVM IR generation
native executable
```

Do not optimize aggressively yet.

Exit:

-   examples build;
-   deterministic output where practical;
-   cross-platform CI.

## Phase 12 --- Concurrency

Add:

``` text
parallel
task
channel
transaction
idempotency
```

Then:

``` text
ownership
shared state
distributed ordering
```

Exit:

-   concurrency test suite;
-   race/lost-update fixtures;
-   database transaction fixtures.

## Phase 13 --- Security

Add:

``` text
secret type
authorization
input provenance
data exposure
unsafe capability
```

Exit:

-   security fixture suite;
-   secret leakage tests;
-   authorization boundary tests.

## Phase 14 --- Benchmark

Run the same tasks with:

``` text
GPT + Go
GPT + Rust
GPT + TypeScript
GPT + Wolio
```

Measure:

``` text
task success
semantic bugs
regressions
repair iterations
compiler feedback loops
tokens
time
blast radius
security defects
```

Do not claim Wolio superiority without these results.

## Phase 15 --- Production hardening

Add:

-   LSP;
-   formatter;
-   package registry;
-   lockfile verification;
-   reproducible builds;
-   compiler compatibility policy;
-   migration tooling;
-   documentation generator;
-   CI integration;
-   observability.

## Phase 16 --- Advanced verification

Only after the baseline proves value:

-   refinement types;
-   property testing;
-   symbolic execution;
-   formal verification;
-   advanced ownership;
-   stronger concurrency proofs.

## Final completion gate

The project is complete when a realistic multi-module application can
be:

1.  initialized;
2.  implemented;
3.  modified by an AI agent;
4.  constrained by scope;
5.  verified;
6.  tested;
7.  built natively;
8.  repaired through structured diagnostics;
9.  audited for security;
10. maintained through multiple AI-generated change cycles.
