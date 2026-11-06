# Complete V1 task contracts

This is the task packet for every work unit after T004. Task IDs are not missing
files: their complete contracts live here. T001–T004 also retain dedicated files.
Read DOCUMENT_AUTHORITY, BASELINE-V1, the listed authority and acceptance family.
Actual readiness and results live only in `.execution/task-status.md`.

All tasks inherit the common checks in ACCEPTANCE_MATRIX. Add fixture inputs and
host test runners in the named areas; record the actual test names and commands
in the execution report. Paths below are bounded responsibility areas: choose
the smallest useful crate layout and record that choice before editing. Do not
invent language semantics to fill an engineering detail.

| Task | Deliverable and bounded paths | Depends on | Authority | Acceptance |
|---|---|---|---|---|
| T020 | resolver/import visibility; crates/wolio-resolver, semantic support | T004 | CORE_V1, INTERFACE_CONTRACTS | SEM resolution/cycles |
| T021 | nominal/record/enum checking; crates/wolio-types and semantic | T020 | CORE_V1 | SEM constructors/types |
| T022 | builtin generics, match and Result handling; types | T021 | CORE_V1, INTERFACE_CONTRACTS | SEM matches/Result |
| T023 | persisted identity map, snapshot graph and register-ids; semantic, CLI | T022 | SEMANTIC_MODEL, INTERFACE_CONTRACTS | ID |
| T024 | interpreter/runtime and run CLI; runtime/interpreter, CLI | T023 | CORE_V1, DELIVERY_POLICY | RUN; all M02 |
| T030 | predicate checker and runtime guards; contracts, interpreter | T024 | CONTRACT_DESIGN | CONTRACT |
| T031 | conservative errors/effects/dependencies; effects, contracts, types | T030 | CONTRACT_DESIGN, CORE_V1 | EFFECT |
| T032 | test discovery/runner, evidence aggregation, verify CLI; verify, CLI | T031 | EVIDENCE_MODEL | EVID |
| T033 | inspect/impact/compatibility; semantic/query, CLI | T032 | SEMANTIC_MODEL, CONTRACT_DESIGN, INTERFACE_CONTRACTS | QUERY |
| T034 | three examples, trial install/use docs, pilot harness/raw report; examples, docs, benchmarks/pilot | T033 | DELIVERY_PLAN | TRIAL; all M03 |
| T040 | JSONL envelopes/replay and queries; agent, CLI | T034 | LOCAL_PROTOCOL_V1 | AGENT versions/query/replay |
| T041 | all structured operation plans/canonical diffs; agent | T040 | LOCAL_PROTOCOL_V1, SEMANTIC_MODEL | AGENT plan and old/new ID mapping |
| T042 | scope/capability/approval validation; agent | T041 | LOCAL_PROTOCOL_V1 | AGENT deny/traversal/expiry/revocation |
| T043 | staged verify, lock/CAS, journal/recovery; agent, filesystem adapter | T042 | LOCAL_PROTOCOL_V1 | AGENT crash/concurrent-edit preservation |
| T044 | bounded repair/finalize and evidence binding; agent | T043 | LOCAL_PROTOCOL_V1, EVIDENCE_MODEL | AGENT repair exhaustion/finalize; all M04 |
| T050 | typed IR and structural verifier; IR | T034 | DELIVERY_POLICY, CORE_V1 | NATIVE invalid IR |
| T051 | pin LLVM, checked lowering/object emission; codegen, toolchain config | T050 | DELIVERY_POLICY | NATIVE arithmetic/branches |
| T052 | document ABI and implement runtime linking; runtime, codegen | T051 | DELIVERY_POLICY | NATIVE records/enums/strings/console/guards |
| T053 | build CLI and full differential release suite; CLI, tests/native | T052 | CORE_V1, CONTRACT_DESIGN | NATIVE all; all M05 |
| T060 | frozen expanded corpus/independent hidden tests; benchmarks/corpus | T044 | DELIVERY_PLAN | BENCH corpus and 24-task eligibility |
| T061 | Go/Rust/TypeScript baseline runners; benchmarks/runners | T060 | DELIVERY_PLAN | BENCH equivalent capabilities and budgets |
| T062 | 10 paired runs per arm with raw records; benchmarks/results | T061 | DELIVERY_PLAN | BENCH complete runs incl. failures |
| T063 | statistical/cost analysis and report; benchmarks/analysis, docs | T062 | DELIVERY_PLAN | BENCH intervals/negative findings; all M06 |
| T070 | Git resolution/manifest/lock and package CLI; package, resolver, CLI | T044, T053 | PACKAGE_MANAGER_SPEC, INTERFACE_CONTRACTS | PKG graph/conflicts/imports |
| T071 | credentials/integrity/cache/offline; package | T070 | PACKAGE_MANAGER_SPEC | PKG tamper/offline; all PKG |
| T072 | canonical comment-preserving formatter/fmt CLI; format, parser, CLI | T071 | DELIVERY_POLICY | TOOL formatting |
| T073 | incremental LSP/rename and stale edit checks; LSP, query | T072 | DELIVERY_POLICY, SEMANTIC_MODEL | TOOL editor |
| T074 | Linux artifacts/checksums/license/install docs and CI; release, docs, workflows | T073 | DELIVERY_POLICY | TOOL clean install; all M07 |

## Verification beyond common Rust checks

- T034: execute every documented trial command from a fresh directory; run the
  six-task, three-run-per-language pilot defined in DELIVERY_PLAN. Publish raw
  failures and model/tool IDs; missing access is BLOCKED, not simulated evidence.
- T043: fault-inject every journal/publication boundary and restart recovery.
- T053: run the conformance corpus under both backends and compare normalized
  outputs, faults and violated clause identities with release optimization on.
- T060–T063: use the frozen benchmark protocol, retain corpus/run hashes and
  reproducible runner/analysis commands in the report. Model API access/cost is
  an execution prerequisite, not permission to fabricate benchmark runs.
- T071: run isolated local Git fixtures online and disconnected; capture exact
  add/install/update/locked/offline invocation results.
- T074: install the built archive in a fresh Linux x86_64 environment, validate
  checksums and run the documented sample. Record the selected project license
  from the owner's decision; never invent ownership or a license grant.

## Report contract

Use `.execution/reports/Tnnn.md`: task ID, baseline identity, changed paths,
implemented behavior, commands/exit results, fixture families, limitations and
follow-up. DONE requires every acceptance item. A blocked command is not passed.
Choose the first eligible task when asked to continue; do not silently execute
later tasks when the user assigns only one ID.
