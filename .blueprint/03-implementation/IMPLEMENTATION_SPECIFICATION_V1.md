`.blueprint/03-implementation/IMPLEMENTATION_SPECIFICATION_V1.md`
# Wolio implementation specification V1

Status: revision 1.3 freeze candidate. V1 denotes product scope, not milestone 1.
This specification covers the whole M01–M07 delivery. There is no V2 baseline yet.

## Pipeline and responsibilities

```text
source -> lexer -> parser/AST -> resolver -> shared semantic model
       -> types -> contract well-formedness -> errors/effects/dependencies
       -> verification/query services
       -> interpreter (M02; contract guards M03)
        -> typed IR/verifier -> VerifiedWolioIr -> LLVM/native runtime (M05)
```

Diagnostics are low-level shared values. The CLI composes services; it does not
implement language semantics. Agent, LSP and formatter consume the same compiler
model. Runtime never depends on LLVM. Cargo edges are defined in DELIVERY_POLICY.
Create only useful crates, not every historical crate sketch at T001. Interpreter
and diagnostic modules may become separate crates when required by boundaries.

## Language and enforcement

CORE_V1 is the sole accepted grammar: immutable nominal/record/enum values,
operations and implementations, explicit Result/Option and exhaustive match.
No user recursion, mutation, arbitrary generics or stateful invariants in V1.
Console output is the only host adapter. Contracts are runtime guarded;
well-formedness, errors, effects and dependencies are checked statically.
Unknown implication is never proof. Unsupported profile features fail explicitly.

## Delivery

| Stage | Output | Work units |
|---|---|---|
| M01 | init/check frontend and diagnostics | T001–T004 |
| M02 | semantic graph, types, interpreter/run | T020–T024 |
| M03 | contracts, tests/verify, inspect/impact and pilot trial | T030–T034 |
| M04 | local scoped agent protocol and recovery | T040–T044 |
| M05 | typed IR, LLVM, ABI and parity | T050–T053 |
| M06 | controlled expanded benchmark | T060–T063 |
| M07 | Git packages, formatter, LSP and distribution | T070–T074 |

M04 and M05 independently follow M03. M06 follows M04; M07 follows M04 and M05.
V1 is complete only after all stages pass their gates. M03 is independently
launchable. Package resolution, formatter and LSP are not trial prerequisites.

## Contracts between components

Use INTERFACE_CONTRACTS for project layout, command behavior and shared formats;
SEMANTIC_MODEL for identity; EVIDENCE_MODEL for evidence; LOCAL_PROTOCOL_V1 for
mutation; PACKAGE_MANAGER_SPEC for dependency integrity. Never expose sketches
from historical crate documents as a competing public contract.

All acceptance cases are indexed in `../05-quality/ACCEPTANCE_MATRIX.md` and
assigned to work units in `../04-tasks/task-index.md` and their task files under `../04-tasks/`. A task is complete
only with executed checks and recorded evidence. Written specifications alone do
not validate parser correctness, runtime parity or benchmark claims.
