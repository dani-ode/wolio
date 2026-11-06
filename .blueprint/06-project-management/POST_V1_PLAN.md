`.blueprint/06-project-management/POST_V1_PLAN.md`
# Post-V1 product plan

This plan covers the rest of the vision without assigning fictional language
versions. All items start PLANNED (design/research); none is READY for compiler
implementation. A successful experiment produces a proposed versioned baseline,
syntax/semantics, migration policy, threat model, conformance cases and task set.
Owner acceptance is required before merging new language behavior.

| ID | Work package | Prerequisites | Required design and exit evidence |
|---|---|---|---|
| E01 | Stateful resources and invariant frames | M03, M05 | Choose ownership/checked-access model; define aliases, reads/writes, old snapshots and lifecycle checks; reject alias misuse and prove unrelated state unchanged in fixtures |
| E02 | Transactions and adapters | E01 | Define adapter isolation/commit/rollback and error-state guarantees; fault injection around commit; explicitly reject cross-adapter atomicity claims |
| E03 | Structured concurrency | E01 | Define spawn/join/cancel, ordering, errors and lifetime; deterministic scheduler tests, race/lost-update cases and cancellation cleanup |
| E04 | Durable idempotency | E02, E03 | Define keys, fingerprints, expiry, persistence and conflicts; concurrent duplicates and crash/retry tests |
| E05 | Validation provenance and secrets | M03; E01 for resource handles | Define untrusted/validated nominal transitions and opaque secret sinks; reject implicit trust, literals, disclosure, display and serialization; redaction tests |
| E06 | Expanded architecture policy and advisory analyzer | M04, M06 | Version allow/deny graph rules with deny precedence; separate rule violation from heuristic suspicion; transitive-edge fixtures and false-positive evaluation. M04 already enforces the V1 anti-workaround baseline defined by LOCAL_PROTOCOL_V1 and DIAGNOSTIC_SPEC. |
| E07 | Properties, refinements, bounded proof | M03, M06 | Specify generators/shrinking/replay; define supported proof fragment, solver pin, timeout and unknown; counterexample and timeout tests; never equate unknown with proof |
| E08 | FFI and external I/O adapters | M05, E01; E05 for secrets | Define ABI, ownership, fault boundary, effects and capability policy; invalid ABI/lifetime and unauthorized I/O tests |
| E09 | Remote collaboration and isolation | M04, E05 | Define authenticated identities, authorization/revocation, multi-user locks and OS isolation; replay, cross-user and escape testing |
| E10 | Additional language expressiveness | M06 and demonstrated user need | Evaluate generics, traits, inheritance, macros, events and entities individually; supply desugaring/semantics, diagnostics, cost and migration experiments before admission |

No date or release number is promised. Prioritize only after the trial feedback
and measured limitations. E10 is an explicit decision backlog, not approval of
all listed features. An extension may be rejected with rationale while V1 remains
complete. Update PROJECT_COVERAGE whenever an item is accepted, split or rejected.
