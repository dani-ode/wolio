`.blueprint/CHANGELOG.md`
# Blueprint Changelog

> Non-normative history of blueprint revisions. Active rules are defined by
> [DOCUMENT_AUTHORITY.md](DOCUMENT_AUTHORITY.md) and its binding documents.

## Unreleased

### Revision 1.5 — 2026-11-14 (current freeze candidate)

- Addressed industrial vibe-coding and systems architecture review:
  - **Forward-Reserved Keywords:** Added `with_mock`, `where`, `fn`, and `foreign` to V1 Set 3 forward-reserved keywords (23 total keywords) across `CORE_V1.md`, `INTERFACE_CONTRACTS.md`, and `T002`.
  - **LLM Mutation Ergonomics:** Solved JSON-AST verbosity paradox in `MUTATION_SCHEMA_V1.md` and `LOCAL_PROTOCOL_V1.md` by allowing canonical Wolio `code: string` payloads, parsed server-side into typed AST diffs by `wolio-parser`.
  - **Early Anti-Hardcoding Defense:** Added scalar boundary and property fuzzing in M03 (`wolio verify --fuzz`) with `WOLIO-VERIFY-002` to defeat test-overfitting on scalar types directly in V1.
  - **M06 Rescoping:** Refined V1 M06 evaluation from a 960-run algorithmic matrix to a 10-task Contract & Protocol Benchmark (200 runs), reserving the 24-task algorithmic benchmark for V2 M11 (`T215`).
  - **Crate Consolidation:** Merged compiler middle-end into `crates/wolio-semantic` (submodules `resolver`, `types`, `graph`, `contracts`, `effects`, `analyzer`), reducing workspace crates from 19 to 14 and eliminating inverted dependency hazards.
  - **Pure Rust Native Codegen:** Pinned `wolio-codegen` to verified textual LLVM IR (`.ll`) emission and `clang` compilation, eliminating brittle Cargo `llvm-sys` / `inkwell` C++ build risks.
  - **Conformance Fixture Header Specification:** Standardized test execution directives (`// @mode:`, `// @expect-status:`, `// @expect-code:`) for all `tests/conformance/` fixtures.
  - **Single Normative Authority:** Designate `04-tasks/Tnnn-*.md` as the sole normative source for all tasks, eliminating dual-source hazards with summary indexes.
  - **Compact Systems Strategy:** Incorporated full systems roadmap across V1 (scalar & IR foundation), V2 (bitwise ops, exact math), and V3 (capability-gated FFI & fixed-width integers), strictly avoiding a separate V4.
  - **V2 Turbofish Syntax:** Resolved Pratt parser `<` comparison ambiguity by establishing explicit turbofish syntax (`List::empty::<T>()`, `Map::empty::<K, V>()`) and declaration-target type inference.
  - **V2 Checked Bitwise Operations:** Added functional checked bitwise operations (`bit_and`, `bit_or`, `bit_xor`, `bit_shl`, `bit_shr`, `bit_not`) for `Int` and `UInt` with strictly bounded shift amounts (`0 <= n < 64`).
  - **V2 Exact Decimal with Explicit Rounding:** Enforced strict trapping on inexact unscaled remainders for standard `*` and `/`, while providing explicit audited rounding via `Decimal::mul_round` and `Decimal::div_round` with `RoundingMode` enum (`HalfEven`, `HalfUp`, `Floor`, `Ceil`, `Truncate`).
  - **V2 14-Crate Architectural Alignment:** Synchronized `IMPLEMENTATION_SPECIFICATION_V2.md` and all V2 task contracts (`T201`–`T216`) to the consolidated 14-crate DAG (`crates/wolio-semantic` middle-end) and pure Rust textual LLVM IR emission.
  - **V2 Full Algorithmic Benchmark Allocation:** Pinned the 24-task × 10-seed × 4-language (960 runs) comparative algorithmic benchmark to M11 (`T215`).
  - **V3 Capability-Gated Foreign Function Interface (FFI):** Added `foreign type` (linear, opaque, non-storable `WOLIO-FOREIGN-002`, scope auto-drop glue) and `foreign operation` (contract-guarded interop via compiler-generated C glue) with manifest gating in `wolio.toml [foreign].allowed_libraries` (`WOLIO-FOREIGN-001`, `HUMAN_REQUIRED`) and mandatory test mocking (`Foreign::Call`, `WOLIO-EFFECT-002`).
  - **V3 Fixed-Width Integer Scalar Types:** Added `UInt8`..`UInt32` and `Int8`..`Int32` with checked arithmetic (`WOLIO-RUNTIME-001`), explicit checked conversions (`UInt8::from_uint`), zero silent truncation, and native LLVM IR mapping (`i8`, `i16`, `i32`).
  - **V3 Linear Resource Instantiation Deadlock Resolution:** Defined scope-bound acquisition `Resource::acquire::<T>(...)` at entrypoints and `Resource::mock::<T>()` in `with_mock` test blocks, with strict non-aliasing (`WOLIO-RESOURCE-001`), `modifies` frames, and `old(...)` pre-state snapshots.
  - **V3 14-Crate Alignment & Task Expansion:** Synchronized `IMPLEMENTATION_SPECIFICATION_V3.md` and all V3 tasks to the 14-crate DAG (`crates/wolio-semantic` middle-end) and pure Rust textual LLVM IR emission; added tasks `T316`–`T319` in Milestone M14 for FFI and fixed-width types, completing V3 with 19 self-contained tasks (`T301`–`T319`).

### Revision 1.4 — 2026-11-10 (superseded by revision 1.5)

- Incorporated [ADR-008](01-architecture/decisions/ADR-008-multi-version-anti-workaround-roadmap.md):
  - Removed all 46 superseded historical draft files (`LANGUAGE_SPEC.md`, `SCOPE.md`, `WTP_SPEC.md`, 18 crate sketches in `03-implementation/crates/`, 7 milestone drafts in `03-implementation/milestones/`, and `M00-*` reports) to eliminate conflicting syntax and error codes.
  - Reserved all V2 and V3 keywords in the V1 lexer (`CORE_V1.md`, `INTERFACE_CONTRACTS.md`, `T002`) to guarantee zero source collision across V1, V2, and V3.
  - Resolved all remaining task `Open:` items in `T020` (`WOLIO-RESOLVE-001` for import/call cycles) and `T024` (declarative `effects`/`dependencies`/`errors`/`intent` permitted in M02; only `requires`/`ensures` trigger `WOLIO-FEATURE-001` at M02 `run`).
  - Strengthened `WOLIO-ANALYZER-001`..`005` in `DIAGNOSTIC_SPEC.md` and `T044` so `002` (swallowed error returning constant `Ok`), `003` (unused declared dependency/effect padding), `004` (constant-folded forbidden literals), and `005` (identical arms and test-literal mirroring) are concrete, enforceable checks.
  - Added complete, non-colliding blueprints for **V2** ([BASELINE-V2.md](BASELINE-V2.md), [CORE_V2.md](02-specifications/language/CORE_V2.md), [V2-TASKS.md](04-tasks/V2-TASKS.md): `M08`–`M11`, `T201`–`T216`) and **V3** ([BASELINE-V3.md](BASELINE-V3.md), [CORE_V3.md](02-specifications/language/CORE_V3.md), [V3-TASKS.md](04-tasks/V3-TASKS.md): `M12`–`M15`, `T301`–`T315`), including mandatory inter-version Bug-Fix & Regression Gates (`M08` and `M12`).
  - Made task files self-contained and aligned `.execution/baseline-acceptance.md` to Revision 1.4.

### Revision 1.3 — 2026-11-06 (superseded by revision 1.4)

- Incorporated [ADR-007](01-architecture/decisions/ADR-007-deterministic-identity-and-gated-automation.md): removed `wolio.ids.json`, defined deterministic `sym_` SHA-256 declaration identities, required typed-only AST agent mutations, introduced `AUTO`/`POST_REVIEW`/`HUMAN_REQUIRED` risk tiers, made the interpreter the default development backend, and restricted LLVM to `VerifiedWolioIr`.

### Revision 1.2 — 2026-11-06 (superseded by revision 1.3)

- Added authoritative document register, whole-project coverage, shared CLI/manifest/hash contracts, and expanded V1 task contracts `T020`–`T074`.
