# Wolio AI Vibe Coding --- Master Technical Blueprint

**Status:** Pre-implementation baseline\
**Audience:** Compiler engineers, language designers, AI-agent
engineers, verification engineers\
**Primary goal:** Build Wolio as an AI-native software construction
system, not merely as a stricter programming language.

## 1. Executive decision

Wolio must optimize for:

1.  machine verifiability;
2.  explicit semantics;
3.  bounded AI change scope;
4.  deterministic dependency/effect behavior;
5.  semantic traceability;
6.  safe repair loops;
7.  long-term AI maintainability.

The central product is:

``` text
Human Intent
    ↓
Requirement / Specification
    ↓
Wolio Contract
    ↓
AI Change Proposal
    ↓
Scope + Capability Check
    ↓
Semantic Analysis
    ↓
Implementation
    ↓
Verification
    ↓
Executable
```

The language is only one layer of the system.

The design deliberately avoids treating strictness as correctness. A
program can be structurally valid while implementing the wrong business
rule. Therefore Wolio separates:

-   intent correctness;
-   contract correctness;
-   implementation correctness;
-   regression correctness.

## 2. Product boundary

Wolio consists of two tightly integrated products.

### A. Wolio Language Platform

-   `.wol` source language
-   lexer/parser
-   AST
-   semantic model
-   static type checker
-   Result/Option
-   exhaustive matching
-   contracts
-   explicit dependencies
-   explicit effects
-   semantic/domain types
-   compiler
-   Wolio IR
-   LLVM backend
-   native executable
-   package manager
-   formatter
-   test runner
-   LSP

### B. Wolio AI Construction System

-   Agent Protocol
-   semantic repository graph
-   change scopes
-   capabilities
-   intent tracking
-   impact analysis
-   structured mutations
-   verification orchestration
-   repair protocol
-   workaround detection
-   secret protection
-   decision memory
-   AI maintainability analysis

The second layer is the main differentiator from Rust, Go, and similar
languages.

## 3. Non-negotiable principles

### P1 --- No silent behavior

No implicit fallback, undeclared dependency, undeclared effect, hidden
global mutation, or swallowed error.

### P2 --- AI does not edit arbitrary text by default

AI should operate through structured actions such as:

``` text
create_symbol
modify_symbol
modify_contract
add_test
add_dependency
declare_effect
rename_symbol
move_symbol
request_capability
request_scope
```

Raw text editing exists only as a controlled low-level capability.

### P3 --- Contract changes are first-class changes

Changing a precondition, postcondition, invariant, error set, effect
set, or dependency contract is not an ordinary implementation edit.

It creates a contract-change proposal with impact analysis.

### P4 --- Compiler and analyzer have different responsibilities

Compiler must reject illegal programs.

Analyzer should detect likely architectural, semantic, or AI-specific
problems.

AI may propose repairs.

Humans retain authority over contract changes, security exceptions, and
high-risk architecture changes.

### P5 --- No suppression principle

Semantic violations cannot be bypassed using:

``` text
ignore
disable
no-check
lint-disable
unsafe-ignore
```

`unsafe` is permitted only as an explicit, auditable capability.

### P6 --- Semantic identity is independent from filesystem identity

Filesystem conventions may be verified, but paths are not the type
system. Refactoring must remain possible.

### P7 --- Optimize for evidence, not elegance

Every major feature must be justified by benchmarks or real failure
cases.

## 4. MVP scope

### Tier 0 --- mandatory

-   static typing;
-   Result / Option;
-   exhaustive match;
-   explicit dependencies;
-   explicit effects;
-   contracts;
-   semantic IDs;
-   compiler verification.

### Tier 1 --- required for the AI-native value proposition

-   AI Agent Protocol;
-   change scope;
-   capability model;
-   semantic repository graph;
-   impact analysis;
-   structured code mutations;
-   mandatory verification;
-   semantic/domain types;
-   provenance;
-   intent;
-   fallback/workaround detection;
-   secret type.

### Tier 2 --- later

-   ownership;
-   advanced concurrency;
-   architecture contracts;
-   property testing;
-   stronger refinement types;
-   formal verification.

### Deliberately postponed

-   filesystem-as-type-system;
-   line number as identity;
-   mandatory large file trees;
-   hard language-level LOC limits;
-   excessive metadata;
-   advanced formal methods before baseline benchmarks.

## 5. Success criteria

Wolio is successful only if it improves AI software construction against
a baseline such as GPT + Go/Rust/TypeScript.

Measure:

-   task correctness;
-   semantic bugs escaping verification;
-   regression rate;
-   repair iterations;
-   compiler feedback loops;
-   token consumption;
-   wall-clock time;
-   change blast radius;
-   security defects;
-   race/concurrency defects;
-   hidden dependency count;
-   fallback/workaround count;
-   AI maintainability score.

The benchmark must include an AI bug taxonomy:

``` text
W01 Wrong type
W02 Missing error handling
W03 Hidden dependency
W04 Wrong dependency direction
W05 Missing test
W06 Incorrect edge case
W07 Wrong algorithm
W08 Wrong business rule
W09 Race condition
W10 State inconsistency
W11 Security flaw
W12 Contract misunderstanding
W13 Incorrect assumption
W14 Unintended side effect
W15 Regression
```

## 6. Definition of Done for the platform

The platform is not considered finished when `hello world` compiles.

It is finished when:

1.  Wolio projects can be created reproducibly.
2.  Wolio source has a stable grammar.
3.  Compiler diagnostics are structured and machine-readable.
4.  AI can inspect semantic state without reading the whole repository.
5.  AI can request bounded changes.
6.  Out-of-scope edits are rejected.
7.  Contracts cannot be silently weakened.
8.  Effects and dependencies are checked.
9.  tests and verification are mandatory according to project policy.
10. unsafe behavior is auditable.
11. secrets cannot become source literals.
12. semantic graph and decision memory survive refactors.
13. a realistic multi-module project can be built.
14. benchmark results demonstrate improvement over a
    conventional-language baseline.

## 7. Final architecture

``` text
                    HUMAN / PRODUCT INTENT
                              │
                              ▼
                    REQUIREMENT MODEL
                              │
                              ▼
                       WOLIO CONTRACTS
                              │
                    ┌─────────┴─────────┐
                    ▼                   ▼
             SEMANTIC MODEL       AI AGENT PROTOCOL
                    │                   │
                    └─────────┬─────────┘
                              ▼
                     CHANGE PROPOSAL
                              │
                  Scope + Capability Check
                              │
                              ▼
                    Semantic Repository Graph
                              │
              ┌───────────────┼────────────────┐
              ▼               ▼                ▼
          Type Check      Effect Check     Dependency Check
              │               │                │
              └───────────────┼────────────────┘
                              ▼
                      Semantic Analyzer
                              │
                              ▼
                      Wolio Intermediate IR
                              │
                              ▼
                           LLVM
                              │
                              ▼
                         Executable
```

This architecture is the baseline for all other documents in this
blueprint.
