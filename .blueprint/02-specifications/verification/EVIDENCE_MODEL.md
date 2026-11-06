`.blueprint/02-specifications/verification/EVIDENCE_MODEL.md`
# Verification evidence V1

Each obligation records `id`, `symbol`, `clause`, `source_hash`, `tool_version`,
`method`, `status`, and related diagnostic/test IDs. Methods are `static`,
`runtime_guard`, `test`, or `review`. Status is `pass`, `fail`, `not_run`, or
`unknown`. A runtime guard installed successfully is not proof that all executions
pass. Test success applies only to recorded inputs. Review cannot replace a
failed language check. Reports preserve this distinction in JSON and human output.

`check` runs syntax/resolution/types/contracts well-formedness/effects/dependencies.
`test` runs source tests with guards enabled. `verify` combines check, tests, and
policy gates, binding results to the exact source, manifest, lockfile, compiler,
and policy hashes. Missing required evidence fails finalization. Unsupported
checks are `not_run`, never silently `pass`. Changed inputs invalidate evidence.

Diagnostics retain DIAGNOSTIC_SPEC's shape, with optional `obligation`, `related`,
`evidence`, and `snapshot` fields. Source spans are zero-based UTF-8 byte offsets,
start inclusive/end exclusive; paths are normalized relative to project root.
`target` is null before identity assignment. Sort by path, start, code, target.
Version 1 readers ignore unknown optional fields; changed field meaning requires
a major schema revision. No source secrets or full runtime values in diagnostics.

## Initial code registry

| Code | Meaning |
|---|---|
| WOLIO-SYNTAX-001 | invalid token or syntax |
| WOLIO-RESOLVE-001 | missing, duplicate, or inaccessible symbol |
| WOLIO-TYPE-001 | incompatible types |
| WOLIO-MATCH-001 | missing or duplicate match arm |
| WOLIO-ERROR-001 | undeclared/unhandled error |
| WOLIO-EFFECT-001 | undeclared transitive effect |
| WOLIO-DEPENDENCY-001 | undeclared operation dependency |
| WOLIO-CONTRACT-001 | malformed predicate |
| WOLIO-CONTRACT-002 | requires failed |
| WOLIO-CONTRACT-003 | ensures failed |
| WOLIO-CONTRACT-004 | predicate evaluation fault |
| WOLIO-RUNTIME-001 | arithmetic fault |
| WOLIO-FEATURE-001 | unsupported feature/profile |
| WOLIO-SCOPE-001 | unauthorized mutation |
| WOLIO-STALE-001 | stale source/approval snapshot |
| WOLIO-PACKAGE-INTEGRITY-001 | dependency integrity mismatch |

New codes are additive and never reuse old meanings. Repairs are suggestions,
not executable authority; adding an effect, error, or weakening a guarantee is
a contract change. Exit codes: 0 command success, 1 source/test/runtime rejection,
2 usage/configuration error, 3 internal tool failure. JSON output is one object
with `schema_version`, `command`, `status`, `diagnostics`, and `evidence` arrays;
program output is a separate field for run and never corrupts JSON stdout.
