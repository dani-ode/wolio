# V1 acceptance matrix

Each row is a required fixture family, not a claim tests already exist. Fixtures
must assert observable outputs or rejections, not mirror implementation internals.
Store compiler conformance cases under `tests/conformance/<family>/`; host-side
tests execute them with the real frontend/runtime. Do not add fake passing tests.

| Family | Positive and boundary evidence | Required rejection/failure evidence | Owner |
|---|---|---|---|
| FRONT | all grammar productions, Unicode strings, CRLF byte spans, precedence, empty module | invalid UTF-8/token/escape, unfinished string, malformed declaration, chained comparisons | T002–T004 |
| SEM | imported public symbols, nominal constructors, records, every builtin generic, exhaustive matches | duplicate/private/missing symbols, cycles, nominal mismatch, bad arity, recursive values, missing/duplicate arms | T020–T022 |
| ID | stable IDs on formatting/move, exact rename mapping, cache deletion reproduction | duplicate ID, ambiguous reconciliation, resurrected tombstone, stale snapshot | T023 |
| RUN | Unit/Int entry, selected branches, short-circuiting, checked limits, console capture | overflow, divide zero, MIN/-1, missing entry, unsupported contract profile | T024 |
| CONTRACT | conjunction/source order, nominal inputs, Result postconditions, nested operations | result in requires, predicate calls/type faults, false requires/ensures, predicate overflow | T030 |
| EFFECT | propagated error/effect sets, explicitly handled Result, unused declared error | undeclared Err, hidden transitive effect, missing dependency, discarded Result | T031 |
| EVID | pass/fail/not_run/unknown, deterministic test order, snapshot-bound evidence | changed input invalidates evidence, missing reachable-error/boundary tests, false/faulting tests | T032 |
| QUERY | stable inspect, typed reverse paths, identical predicate compatibility | removed public API, changed nominal/signature/sets; unknown nonidentical predicate implication | T033 |
| TRIAL | three runnable examples, init/check/run/test/inspect/impact/verify, three change cycles | wrong implementation passes types but fails ensures; documented limitations and unsuccessful pilot runs | T034 |
| AGENT | valid scoped edit, all mutation methods, replay, finalization | stale/expired/revoked approval, traversal/symlinks, changed replay, crash at publication steps, concurrent user edits, failed verify | T040–T044 |
| NATIVE | typed IR, stable ABI, release guards, interpreter/native differential corpus | bad IR edges/terminators, matching faults and contract clauses, unsupported target | T050–T053 |
| BENCH | corpus/prompts/hashes/versions, paired runs, raw data and confidence intervals | timeouts/failures retained, no selective omission, no unsupported improvement claims | T060–T063 |
| PKG | deterministic graph, private Git helper, verified cache, locked offline install | conflicting ranges/cycles, moved tags, altered bytes, missing offline data, module collision, escaping symlinks/submodules | T070–T071 |
| TOOL | format idempotence/comment/semantic preservation, LSP hover/definition/references/rename, clean Linux install | fmt check changes nothing, stale editor version, unsaved buffer isolation, corrupt archive/checksum | T072–T074 |

## Common execution gates

Every Rust work unit runs `cargo fmt --all -- --check`,
`cargo clippy --workspace --all-targets -- -D warnings`, and
`cargo test --workspace`. T001 additionally runs `cargo check --workspace`.
Keep tests independent of network except explicitly marked Git integration tests
using controlled repositories. Never require production credentials in CI.
Snapshots omit wall-clock timestamps, random IDs and machine-specific paths where
those are not part of the tested behavior; semantic identity tests use fixed maps.

Milestone acceptance requires all its families and preceding dependencies. M03
trial distribution may use documented Cargo source installation with pinned tools;
M07 additionally requires clean-machine binary artifact installation. Neither
requires positive benchmark findings. Full V1 requires both M06 and M07.
