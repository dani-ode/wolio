# Wolio Implementation Specification v1

**Status:** Draft for implementation  
**Compiler:** Rust  
**Native backend:** LLVM

## Purpose

This document defines the first implementation baseline below the Wolio Blueprint. The Blueprint defines direction and principles; this document defines the components, interfaces, invariants, and verification gates required for v1.

## Product boundary

Wolio v1 combines a `.wol` language, compiler, semantic repository graph, verification engine, package manager, and AI construction protocol. The implementation must prioritize machine-verifiable semantics over language breadth.

## Compiler pipeline

```text
source -> lexer -> parser -> AST -> resolver -> semantic model
       -> type checking -> contract checking -> effect checking
       -> dependency checking -> scope/capability checking
       -> semantic analysis -> Wolio IR -> LLVM IR -> native binary
```

The semantic model is shared by the compiler and AI tooling. Generated graph data is rebuildable from source and explicit project metadata and is never the sole source of truth.

## Language baseline

V1 includes modules, entities, types, enums, errors, operations, implementations, contracts, tests, properties, and events. It provides static typing, `Option<T>`, `Result<T,E>`, semantic/domain types, explicit dependencies, explicit effects, typed errors, provenance, and exhaustive matching.

Contracts describe inputs, preconditions, outputs, postconditions, errors, effects, dependencies, and invariants. Implementations may not silently weaken a contract. Contract changes require an impact report and human approval.

## Verification invariants

- No undeclared dependency or effect.
- No unresolved symbol or non-exhaustive match.
- No semantic suppression mechanism.
- No unauthorized capability or out-of-scope mutation.
- Secrets cannot be initialized from source literals.
- Required tests and diagnostics are machine-readable.
- Fallbacks and workarounds require explicit authorization.

## Workspace and crates

The Rust workspace is implemented in dependency order. Its crate responsibilities are specified under `crates/`. The initial boundaries are lexer, parser, AST, types, semantic model, contracts, effects, resolver, analyzer, verifier, IR, code generation, runtime, package manager, agent protocol, CLI, formatter, and LSP.

## Package management

The package manager is GitHub-first and Git-native. The normative rules are defined in `02-specifications/package-manager/PACKAGE_MANAGER_SPEC.md`. The implementation must support repository identity, SemVer resolution, deterministic dependency graphs, commit and content-hash verification, lockfiles, private repository credentials, and a content-addressed cache.

## AI construction protocol

The agent lifecycle is discover, understand, propose, authorize, mutate, verify, repair, and finalize. Agents receive explicit read/write/create/delete scope and capabilities. Preferred mutations are structured semantic operations; raw text editing is a restricted capability. Human approval is required for contract, security, capability, architecture, schema, and unsafe changes.

## Definition of v1

V1 is complete when a realistic multi-module project can be created reproducibly, parsed, type-checked, contract-checked, effect-checked, dependency-checked, verified, compiled through LLVM, and modified through bounded agent actions with structured diagnostics and passing regression tests.
