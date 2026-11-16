`.blueprint/blueprint.md`
# Wolio — Industrial AI-Native Programming Language Master Overview (V1–V3)

> Normative execution baselines are [BASELINE-V1.md](BASELINE-V1.md),
> [BASELINE-V2.md](BASELINE-V2.md), and [BASELINE-V3.md](BASELINE-V3.md). This
> document summarizes the industrial philosophy unifying all three versions.

## 1. Core thesis: Industrial Vibe Coding and Defeating AI "Akal-Akalan"

Wolio is a purpose-built programming language designed from first principles for
**Industrial Vibe Coding**—a software engineering paradigm where:
1. **The Human is the Architect and Specifier:** The user defines domain contracts,
   behavioral invariants, `intent` specifications, capability boundaries, and
   acceptance tests. The human **does not need to read the generated code**.
2. **The Machine is the Coder and Verifier:** AI generates, mutates, and refactors
   the implementation under mechanical compiler constraints. Human readability
   is explicitly subordinated to 100% deterministic, machine-verifiable safety.
3. **Bugs are Eliminated at Definition Time:** The compiler acts as an unforgiving,
   hyper-aggressive gatekeeper ("lebih garang dari Clippy") that complains instantly
   and refuses to compile any sloppy, ambiguous, unhandled, or reward-hacked logic.

In conventional languages (Python, TypeScript, Go, Rust), AI coding models routinely
introduce subtle technical debt ("akal-akalan") to force code to pass tests:
1. **Swallowing errors:** Catching an error and returning dummy `Ok(0)` to silence types.
2. **Hardcoding to unit tests:** Writing `if (input == 42) { return 10; }` or
   `99 + 1` to pass a specific test without implementing the domain rule.
3. **Unbounded loops and state corruption:** Writing unverified loops or mutating state
   without transactional safety.
4. **Hidden side effects and spaghetti dependencies:** Calling I/O deep inside pure logic.
5. **Weakening contracts or deleting tests:** Relaxing types to force CI to pass.

## 2. Immutable architectural directives

Wolio enforces four non-negotiable architectural directives:

1. **Zero-Unsafe Policy:**
   - Wolio has **NO `unsafe` keyword**, no raw pointers, no unchecked memory
     dereferencing, and no manual allocation/deallocation.
   - All memory is 100% safe by construction (immutable value semantics, CoW
     reference counting, and scope-bound linear resource handles).
   - Foreign interfaces (V3) use capability-gated, opaque handle-only FFI with
     compiler-generated C glue and mandatory sandbox mocks in tests.
2. **Verbosity as an Industrial Machine Ratchet:**
   - Lengthy nominal type definitions, explicit `dependencies [...]`, `effects [...]`,
     `errors [...]`, `requires`, `ensures`, `bound`, and `invariant` clauses are
     intentional. Verbosity costs an AI agent fractions of a second, but gives the
     compiler complete mathematical leverage to detect defects instantly.
3. **Hyper-Aggressive 6-Tier Diagnostic Engine:**
   - Multi-tiered compiler complaints: Tier 0 (`FATAL_SYNTAX`), Tier 1 (`TYPE_RESOLUTION`),
     Tier 2 (`BEHAVIORAL_CONTRACT`), Tier 3 (`SECURITY_CAPABILITY`), Tier 4 (`ANTI_HACKING_ANALYZER`),
     and Tier 5 (`HYPER_STRICT_LINT`).
   - **Zero In-Source Suppression:** Directives like `#[allow(...)]` or `// nolint`
     do not exist. Any attempt to suppress a compiler warning or lint is a fatal error
     (`WOLIO-ANALYZER-001`).
4. **Hermetic & Capability-Gated I/O:**
   - No implicit clocks, randomness, filesystem, or network access. Every external
     effect is tracked transitively and locked down in `wolio.toml`. Test blocks
     run in a strictly hermetic sandbox with mandatory mock tables.

## 3. Three-generation conflict-free architecture (`V1 -> V2 -> V3`)

```text
V1 (M01–M07: Immutable Contract Core & Guarded Agent Protocol)
  - Acyclic immutable core, nominal types, exhaustive Result/Option
  - Runtime-guarded requires/ensures + static errors/effects/dependencies
  - Early scalar boundary & property fuzzing (wolio verify --fuzz) with WOLIO-VERIFY-002
  - Deterministic sym_ graph, local agent protocol with canonical code payloads, analyzer 001..005
  - VerifiedWolioIr -> textual LLVM IR (.ll) lowering + clang native parity, Git package manager, fmt, LSP
  - Forward-reserves all 23 V2 and V3 keywords from T002 (including foreign, with_mock, where, fn)
        │
        ▼
V2 (M08–M11: V1 Bug-Fix Gate + Bounded Loops, Collections, Bitwise & Architecture Policy)
  - M08: Mandatory V1 Bug-Fix & Regression Gate (BUG-nnnn conformance suite)
  - Total bounded loops: for (x in src with acc: T = init) { bound ...; invariant ...; }
  - Immutable List<T>, Map<K,V>, exact Decimal with explicit rounding (mul_round/div_round), UInt, Bytes, format(...)
  - Bitwise operations (bit_and, bit_or, bit_xor, bit_shl, bit_shr, bit_not)
  - Reusable pure acyclic predicate declarations callable in contracts
  - [architecture] allow/deny module dependency rules + multi-module Agent Protocol v2
  - Full 24-task algorithmic comparative benchmark (M11 / T215)
        │
        ▼
V3 (M12–M15: V2 Bug-Fix Gate + Standard Library, Capability FFI, Transactions & Fuzzing)
  - M12: Mandatory V2 Bug-Fix & Copy-on-Write Performance Gate
  - Industrial std::* modules (string, collections, math, json, time, crypto, fs, net)
  - Capability-gated Foreign Module / FFI (foreign type, foreign operation, generated C glue, no raw pointers)
  - Fixed-width integer types (UInt8..UInt32, Int8..Int32) with checked conversions
  - Opaque Secret<T> redaction & validation provenance
  - Capability-gated external effects + mandatory hermetic with_mock in tests
  - Linear non-aliasing resource handles with explicit acquisition scope, modifies/old(...), transaction rollback
  - Contract-driven property fuzzing (wolio verify --fuzz) with shrinking counterexamples
```
