# Wolio --- Pre-Coding Preparation Checklist

Do not start implementation until this checklist is complete.

## 1. Product decisions

Answer and freeze:

-   What exact AI failure modes does Wolio target?
-   Is the primary product the language, the construction system, or
    both?
-   What is V1?
-   What is explicitly out of scope?
-   Who has authority to approve contract changes?
-   What does "safe enough" mean?

Required artifact:

``` text
docs/PRODUCT_SCOPE.md
```

## 2. Failure taxonomy

Create at least the initial taxonomy:

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

Add real examples for each.

Required artifact:

``` text
benchmarks/AI_BUG_TAXONOMY.md
```

## 3. Baseline benchmark

Before Wolio exists, implement a small benchmark suite in conventional
languages.

Use the same AI model and comparable prompts.

Record:

-   success/failure;
-   bugs;
-   iterations;
-   tokens;
-   time;
-   regressions;
-   security issues.

Without a baseline, there is no evidence that Wolio helps.

## 4. Semantic model

Freeze the first semantic vocabulary:

``` text
Module
Entity
Type
SemanticType
Operation
Contract
Implementation
Error
Effect
Dependency
Capability
Intent
Provenance
Invariant
Decision
Test
```

Write relationships between them.

## 5. Compiler error contract

Before coding compiler logic, define the diagnostic schema.

Minimum:

``` json
{
  "code": "WOLIO-TYPE-001",
  "severity": "error",
  "category": "TYPE",
  "target": "semantic_id",
  "message": "...",
  "expected": "...",
  "observed": "...",
  "repairable": true,
  "repair_classes": [],
  "forbidden_repairs": [],
  "impact": {}
}
```

This is critical because compiler output is part of the AI interface.

## 6. Contract grammar

Freeze the V1 grammar for:

``` text
requires
ensures
errors
effects
dependencies
invariants
intent
idempotency
```

Do not begin with formal verification syntax that has not been validated
by real examples.

## 7. Mutation protocol

Define the structured agent API before building the agent.

Minimum operations:

``` text
inspect
create
modify
delete
rename
move
add_test
modify_contract
add_dependency
add_effect
request_capability
request_scope
impact
verify
```

## 8. Scope model

Define:

``` text
READ
WRITE
CREATE
DELETE
EXECUTE
NETWORK
SECRET
MIGRATION
UNSAFE
```

Decide which require human approval.

## 9. Repository layout

Freeze the compiler repository structure before implementation.

Recommended:

``` text
crates/
tests/
fixtures/
examples/
benchmarks/
docs/
```

Do not create dozens of empty crates just for appearance.

## 10. Toolchain

Recommended baseline:

-   Rust stable;
-   Cargo workspace;
-   LLVM;
-   Git;
-   CI;
-   snapshot testing;
-   property testing later;
-   deterministic test environment.

Pin the toolchain version in the repository.

## 11. Build reproducibility

Define:

-   supported OS;
-   supported architectures;
-   compiler version;
-   LLVM version;
-   package lock policy;
-   CI matrix;
-   release artifact format.

## 12. Versioning

Define before V1:

``` text
language version
compiler version
stdlib version
package version
contract version
agent protocol version
diagnostic schema version
```

Do not allow the AI protocol and compiler diagnostics to change
arbitrarily.

## 13. Security threat model

List threats:

``` text
secret leakage
authorization bypass
capability escalation
malicious dependency
supply-chain compromise
unsafe FFI
network exfiltration
data exposure
prompt-induced scope expansion
workaround persistence
```

Define the minimum controls for each.

## 14. Human approval model

Decide:

``` text
AUTO
REVIEW
BLOCK
```

For example:

  Change                        Policy
  ----------------------------- --------------
  implementation inside scope   AUTO
  add test                      AUTO
  add ordinary dependency       REVIEW
  contract change               REVIEW
  secret access                 REVIEW
  security boundary change      REVIEW
  unsafe                        REVIEW/BLOCK
  scope expansion               REVIEW

## 15. Golden fixtures

Create fixtures for:

### Must compile

-   hello world;
-   Result;
-   Option;
-   typed error;
-   explicit dependency;
-   explicit effect;
-   valid contract.

### Must fail

-   undeclared effect;
-   undeclared dependency;
-   invalid semantic type;
-   non-exhaustive match;
-   secret literal;
-   forbidden fallback;
-   out-of-scope mutation;
-   contract weakening;
-   architecture violation;
-   suppression attempt.

## 16. AI benchmark tasks

Prepare 20--50 tasks before claiming success.

Include:

-   CRUD;
-   authentication;
-   payment;
-   idempotency;
-   concurrency;
-   database transaction;
-   API endpoint;
-   authorization;
-   secret configuration;
-   refactor;
-   bug fix;
-   feature addition;
-   regression repair.

## 17. Decision log

Create ADRs before controversial implementation choices.

Minimum:

``` text
ADR-001 Product definition
ADR-002 V1 language scope
ADR-003 Rust compiler implementation
ADR-004 LLVM backend
ADR-005 Semantic ID model
ADR-006 Contract model
ADR-007 Agent Protocol
ADR-008 Scope/capability model
ADR-009 Verification architecture
ADR-010 Concurrency roadmap
```

## 18. First coding session should be tiny

Do not start with the entire compiler.

First milestone:

``` text
wolio init hello
wolio check
```

Then:

``` text
hello.wol
```

parses into a stable AST.

Second milestone:

``` text
wolio check
```

produces a semantic model.

Third:

``` text
wolio build
```

produces a native executable.

Fourth:

``` text
wolio agent plan
```

can inspect and propose a change.

Only after these are stable should the system grow.

## 19. Final pre-coding gate

Start implementation only if all are true:

-   [ ] product scope frozen;
-   [ ] V1 feature list frozen;
-   [ ] out-of-scope list written;
-   [ ] semantic vocabulary defined;
-   [ ] diagnostic schema defined;
-   [ ] agent mutation protocol defined;
-   [ ] scope/capability model defined;
-   [ ] benchmark baseline prepared;
-   [ ] golden fixtures prepared;
-   [ ] threat model prepared;
-   [ ] ADR process established;
-   [ ] Rust/LLVM toolchain pinned;
-   [ ] CI skeleton ready;
-   [ ] repository structure ready.

If any of these are missing, design work should continue instead of
compensating with code.
