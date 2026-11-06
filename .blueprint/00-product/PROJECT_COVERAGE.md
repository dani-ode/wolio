`.blueprint/00-product/PROJECT_COVERAGE.md`
# Whole-project coverage

The trial is a release checkpoint, not the entire project. Every product area is
classified below. "Post-V1" means a planned investigation with explicit admission
criteria, not a feature secretly required by a V1 task.

| Area | Delivery | Authority / acceptance |
|---|---|---|
| UTF-8 source, grammar, AST, diagnostics, project initialization | M01 | CORE_V1; FRONT cases |
| Modules, resolution, public visibility, nominal/record/enum types | M02 | CORE_V1; SEM cases |
| Result/Option, exhaustive matching, checked arithmetic, interpreter | M02 | CORE_V1; RUN cases |
| Stable IDs and reproducible semantic graph | M02 | SEMANTIC_MODEL; ID cases |
| Preconditions/postconditions, typed errors, effects/dependencies | M03 | CONTRACT_DESIGN; CONTRACT cases |
| Tests, evidence, inspect, compatibility and impact | M03 | EVIDENCE_MODEL; QUERY/EVID cases |
| Three runnable examples, user instructions, pilot benchmark | M03 trial | DELIVERY_PLAN; TRIAL cases |
| Intent/change provenance, bounded edits, owner approvals, repair | M04 | LOCAL_PROTOCOL_V1; AGENT cases |
| Atomic publication, recovery and decision/audit records | M04 | LOCAL_PROTOCOL_V1; AGENT cases |
| Typed IR, native backend, runtime ABI, interpreter parity | M05 | DELIVERY_POLICY; NATIVE cases |
| Controlled comparative evaluation and AI bug measurements | M06 | DELIVERY_PLAN; BENCH cases |
| Git package graph, credentials, integrity, lock/cache/offline | M07 | PACKAGE_MANAGER_SPEC; PKG cases |
| Formatter, LSP, installation and versioned distribution | M07 | DELIVERY_POLICY; TOOL cases |
| Stateful resources, frames, lifecycle invariants, transactions | Post-V1 E01–E02 | POST_V1_PLAN |
| Structured concurrency, cancellation, idempotency | Post-V1 E03–E04 | POST_V1_PLAN |
| Value provenance, validation states, secret handles | Post-V1 E05 | POST_V1_PLAN |
| Architecture constraints and anti-workaround baseline | M04 | LOCAL_PROTOCOL_V1; DIAGNOSTIC_SPEC; AGENT cases |
| Expanded architecture policy and advisory AI-debt analysis | Post-V1 E06 | POST_V1_PLAN |
| Property generation, refinements and optional formal proof | Post-V1 E07 | POST_V1_PLAN |
| FFI and external resource adapters | Post-V1 E08 | POST_V1_PLAN |
| Remote/multi-user agent transport and stronger isolation | Post-V1 E09 | POST_V1_PLAN |
| Arbitrary generics, traits/inheritance, macros, events, mutable entities | Uncommitted E10 | POST_V1_PLAN; no accepted syntax |

Acceptance IDs are defined in `../05-quality/ACCEPTANCE_MATRIX.md`. Exact Rust API
layout is an engineering choice; observable language/protocol behavior is not.
The accepted target is Linux x86_64. Other platforms require a support amendment.

## Completion definitions

- M03 trial: frontend + executable core + verification + runnable documentation
  + pilot evidence; no dependency on native code or agent automation.
- V1 complete: all M01–M07 accepted, including M06 evaluation. M07 and M06 may
  finish in either order once their own dependencies are satisfied.
- Long-term vision: a maintained portfolio of extensions, not a finite claim
  that every future programming-language feature has been specified or proved.

Performance and correctness improvement are measured outcomes, not assumed
requirements that permit hiding unsuccessful benchmark results.
