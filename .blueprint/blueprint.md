`.blueprint/blueprint.md`
# Wolio — Industrial AI-Native Programming Language Master Overview (V1–V3)

> Normative execution baselines are [BASELINE-V1.md](BASELINE-V1.md),
> [BASELINE-V2.md](BASELINE-V2.md), and [BASELINE-V3.md](BASELINE-V3.md). This
> document summarizes the industrial philosophy unifying all three versions.

## 1. Core thesis: Defeating AI "Akal-Akalan" (Reward Hacking) at Scale

AI coding models generate code orders of magnitude faster than human programmers,
but in conventional languages (Python, TypeScript, Go, Rust) they frequently
resolve compiler or test failures using shortcuts ("akal-akalan") that accumulate
severe technical debt:
1. **Swallowing errors:** Catching or matching an error and returning a dummy
   success value (`Ok(0)`) to silence the compiler.
2. **Hardcoding to unit tests:** Writing `if (input == 42) { return 10; }` or
   `99 + 1` to pass a specific unit test without implementing the domain rule.
3. **Unbounded loops and state corruption:** Writing `while (true)` loops without
   termination bounds or mutating shared state before an error occurs.
4. **Hidden side effects and spaghetti dependencies:** Calling I/O deep inside a
   helper or importing infrastructure directly into domain logic.
5. **Weakening contracts or deleting tests:** Relaxing type/contract rules or
   removing failing tests so a broken implementation passes CI.

## 2. Strategic design trade-off

Wolio explicitly optimizes for **industrial AI correctness at scale**, not
manual human typing speed or small-script token brevity:
- Verbose, explicit declarations (`dependencies [...]`, `effects [...]`,
  `errors [...]`, `requires`, `ensures`, `bound`, `invariant`, nominal wrappers)
  are intentional.
- Every extra line of explicit contract acts as a mechanical ratchet that
  prevents an AI agent from introducing silent regressions across thousands of
  modules.

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
