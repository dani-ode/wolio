# Complete V1 delivery plan

Task status is execution data in `.execution/task-status.md`. The following is
the frozen work breakdown and acceptance contract, not a claim of completion.

| Milestone | Ordered work units | Acceptance and verification |
|---|---|---|
| M01 | T001 workspace/toolchain; T002 lexer; T003 grammar/AST; T004 JSON CLI/init | cargo fmt --check; cargo clippy --workspace -- -D warnings; cargo test --workspace; valid/invalid frontend fixtures |
| M02 | T020 resolver; T021 nominal/record/enum types; T022 Result/Option/match; T023 identities/query graph; T024 interpreter/run | execute pure examples; reject bad types/cycles; delete cache and reproduce graph; boundary arithmetic faults |
| M03 | T030 predicate checking/guards; T031 errors/effects/dependencies; T032 test/verify runner; T033 compatibility/impact; T034 pilot | all contract conformance cases; success/error/boundary tests; raw pilot; three sequential change cycles |
| M04 | T040 envelopes/query; T041 plans/diffs; T042 authorization/scope; T043 transactional apply/journal; T044 repair/finalize | complete LOCAL_PROTOCOL_V1 acceptance suite, including interrupted publication |
| M05 | T050 typed IR/verifier; T051 LLVM lowering; T052 runtime ABI; T053 parity/release build | native and interpreter agree on outputs/fault categories/contract clauses for all conformance fixtures |
| M06 | T060 expanded corpus; T061 baseline runners; T062 repeated paired trials; T063 analysis/report | published prompts, hashes, model/tool versions, raw runs and uncertainty; no selective omission |
| M07 | T070 Git resolution/lock; T071 integrity/cache/offline; T072 formatter; T073 LSP; T074 distribution | moved-tag rejection; locked offline reproduction; format idempotence; stale editor tests; clean-machine install |

Each work unit inherits relevant normative documents and the milestone tests.
Before execution, expand the work unit using TASK_TEMPLATE with affected paths,
exact commands and fixtures. This is scheduling detail, not permission to invent
semantics. Missing semantic decisions require a baseline amendment.

## Trial stop point

M03 must ship three runnable examples: bounded discount calculation, nominal-ID
lookup returning typed errors, and a multi-operation order total. Include a
deliberately wrong implementation that passes type checking but fails ensures,
and a contract change with its affected callers/tests. A fresh user can init,
check, run, test, inspect, impact, and verify without LLVM or agent orchestration.
Documentation shows actual limitations. Pilot performance may be negative;
the trial is accepted based on reproducible execution, not a marketing win.

## Benchmark protocol

Pilot: 6 tasks (implement, edge-case fix, wrong nominal ID, missing error branch,
contract change, multi-operation regression), 3 runs per language. Compare Wolio
and TypeScript with strict checking and ordinary test tooling; Go/Rust are optional
additional pilot arms. Expanded M06: 24 tasks, 10 paired seeds/runs per arm,
Wolio/Go/Rust/TypeScript, restricted to supported equivalent V1 capabilities.
Concurrency/database/secret tasks are excluded until their extension exists.

Freeze corpus, hidden independent acceptance tests, prompts, budget (3 repair
attempts, 10 minutes/task), model identifier, sampling settings, tool versions,
hardware and pricing before any measured run. Same model/settings and comparable
documentation allowance per arm; record training familiarity as a limitation.
No hidden tests in agent context. Randomize run order. Failures/timeouts count.
Report success proportion, escaped defects, regressions, repairs, tokens, time,
and cost, with paired bootstrap 95% intervals over tasks (10,000 resamples).
Pilot is descriptive only. An improvement claim requires positive lower bound
for paired correctness difference and median cost/time no more than 2x baseline;
otherwise report tradeoffs or inconclusive evidence. Publish raw results and
avoid extrapolating to unsupported applications or all AI models.
