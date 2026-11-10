`.blueprint/04-tasks/task-index.md`
# Task index

Register of every allocated V1 task ID. Read DOCUMENT_AUTHORITY and BASELINE-V1
first, then [EXECUTION_CONTRACTS.md](EXECUTION_CONTRACTS.md) for shared rules.
Each task file is the complete contract; the `Depends on` column mirrors it and
must match. Live task states belong only to `.execution/task-status.md`.

## M01 — Frontend (entry: owner baseline acceptance)

| ID | Task file | Depends on | Acceptance |
|---|---|---|---|
| T001 | [T001-workspace-skeleton.md](T001-workspace-skeleton.md) | owner baseline acceptance | common gates |
| T002 | [T002-minimal-lexer.md](T002-minimal-lexer.md) | T001 | FRONT lexical |
| T003 | [T003-minimal-parser-ast.md](T003-minimal-parser-ast.md) | T002 | FRONT grammar |
| T004 | [T004-diagnostics-cli.md](T004-diagnostics-cli.md) | T003 | FRONT CLI; all M01 |

## M02 — Executable core (entry: M01)

| ID | Task file | Depends on | Acceptance |
|---|---|---|---|
| T020 | [T020-resolver-visibility.md](T020-resolver-visibility.md) | T004 | SEM resolution/cycles |
| T021 | [T021-nominal-record-enum-types.md](T021-nominal-record-enum-types.md) | T020 | SEM constructors/types |
| T022 | [T022-generics-match-result.md](T022-generics-match-result.md) | T021 | SEM matches/Result |
| T023 | [T023-semantic-identity-graph.md](T023-semantic-identity-graph.md) | T022 | ID |
| T024 | [T024-interpreter-run.md](T024-interpreter-run.md) | T023 | RUN; all M02 |

## M03 — Trial (entry: M02)

| ID | Task file | Depends on | Acceptance |
|---|---|---|---|
| T030 | [T030-contract-predicates-guards.md](T030-contract-predicates-guards.md) | T024 | CONTRACT |
| T031 | [T031-errors-effects-dependencies.md](T031-errors-effects-dependencies.md) | T030 | EFFECT |
| T032 | [T032-test-runner-evidence-verify.md](T032-test-runner-evidence-verify.md) | T031 | EVID |
| T033 | [T033-inspect-impact-compatibility.md](T033-inspect-impact-compatibility.md) | T032 | QUERY |
| T034 | [T034-trial-examples-pilot.md](T034-trial-examples-pilot.md) | T033 | TRIAL; all M03 |

## M04 — Agent (entry: M03)

| ID | Task file | Depends on | Acceptance |
|---|---|---|---|
| T040 | [T040-agent-envelopes-replay.md](T040-agent-envelopes-replay.md) | T034 | AGENT versions/query/replay |
| T041 | [T041-typed-plans-ast-diffs.md](T041-typed-plans-ast-diffs.md) | T040 | AGENT plan/raw-text/ID mapping |
| T042 | [T042-scope-approval-tiers.md](T042-scope-approval-tiers.md) | T041 | AGENT deny/traversal/tier/expiry/revocation |
| T043 | [T043-transactional-apply-recovery.md](T043-transactional-apply-recovery.md) | T042 | AGENT crash/concurrent edits |
| T044 | [T044-repair-finalize-review-analyzer.md](T044-repair-finalize-review-analyzer.md) | T043 | AGENT repair/review; ANALYZER; all M04 |

## M05 — Native (entry: M03, independent of M04)

| ID | Task file | Depends on | Acceptance |
|---|---|---|---|
| T050 | [T050-typed-ir-verifier.md](T050-typed-ir-verifier.md) | T034 | NATIVE invalid IR |
| T051 | [T051-llvm-lowering.md](T051-llvm-lowering.md) | T050 | NATIVE arithmetic/branches |
| T052 | [T052-runtime-abi-linking.md](T052-runtime-abi-linking.md) | T051 | NATIVE records/enums/strings/console/guards |
| T053 | [T053-native-build-parity.md](T053-native-build-parity.md) | T052 | NATIVE all; all M05 |

## M06 — Evaluation (entry: M04)

| ID | Task file | Depends on | Acceptance |
|---|---|---|---|
| T060 | [T060-benchmark-corpus.md](T060-benchmark-corpus.md) | T044 | BENCH corpus/eligibility |
| T061 | [T061-baseline-runners.md](T061-baseline-runners.md) | T060 | BENCH capabilities/budgets |
| T062 | [T062-paired-benchmark-runs.md](T062-paired-benchmark-runs.md) | T061 | BENCH complete runs |
| T063 | [T063-benchmark-analysis-report.md](T063-benchmark-analysis-report.md) | T062 | BENCH intervals/negative findings; all M06 |

## M07 — Toolchain (entry: M04 and M05)

| ID | Task file | Depends on | Acceptance |
|---|---|---|---|
| T070 | [T070-git-resolution-lock.md](T070-git-resolution-lock.md) | T044, T053 | PKG graph/conflicts/imports |
| T071 | [T071-package-integrity-offline.md](T071-package-integrity-offline.md) | T070 | PKG tamper/offline; all PKG |
| T072 | [T072-formatter.md](T072-formatter.md) | T071 | TOOL formatting |
| T073 | [T073-language-server.md](T073-language-server.md) | T072 | TOOL editor |
| T074 | [T074-release-distribution.md](T074-release-distribution.md) | T073 | TOOL clean install; all M07 |

Only the IDs above are allocated. Reserve numbers and the milestone numbering
blocks are defined in EXECUTION_CONTRACTS. Future extensions E01–E10 are design
work packages in POST_V1_PLAN, not executable V1 tasks.
