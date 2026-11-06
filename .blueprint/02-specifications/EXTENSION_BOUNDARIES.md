`.blueprint/02-specifications/EXTENSION_BOUNDARIES.md`
# Extension boundaries beyond V1

These decisions complete the product boundary; they do not authorize extra V1
syntax. An extension needs a versioned language amendment and conformance suite.

| Extension | Required semantics before admission | Required evidence |
|---|---|---|
| Stateful resources | explicit read/write handles, non-aliasing ownership or checked access, modifies frame, entry snapshots, invariant entry/exit points | unrelated state unchanged; alias attempt rejected |
| Transactions | adapter-defined atomic boundary, rollback guarantees, isolation, error-state guarantees | fault injection before/after commit; no claim across unrelated adapters |
| Idempotency | key scope, request fingerprint, durable outcome, conflict/expiry behavior | duplicate, concurrent duplicate, crash/retry cases |
| Concurrency | structured child tasks, join/cancellation/error propagation, no detached tasks by default | deterministic scheduling tests; race/lost-update cases |
| Provenance | nominal untrusted/validated states with explicit constructors; no magic trust inference | unvalidated value rejected at trusted boundary |
| Secrets | opaque handles, no literal/display/serialization, explicit disclosure capability | redaction and unauthorized sink tests |
| Formal proof | decidable supported fragment, solver/version/time limits, counterexamples, unknown result | timeout is never proof; runtime fallback remains explicit |
| FFI | ABI, ownership, exception/fault boundary, effect declaration, audited capability | invalid ABI/resource lifetime tests |
| Architecture rules | module edge allow/deny data; deny precedence | forbidden transitive dependency fixtures |

No general theorem can establish business-intent correctness. Heuristic
workaround detection remains advisory; only an explicit violated rule blocks
legality. A future language release may reject an extension after experiments;
V1 remains a complete, independently useful specification without it.
