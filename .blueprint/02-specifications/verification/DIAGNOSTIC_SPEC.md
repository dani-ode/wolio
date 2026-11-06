`.blueprint/02-specifications/verification/DIAGNOSTIC_SPEC.md`
# Diagnostic Schema

Diagnostics are part of the AI interface, not just compiler text.

## Canonical shape

``` json
{
  "schema_version": 1,
  "code": "WOLIO-EFFECT-001",
  "severity": "error",
  "category": "effect",
  "target": "sym_<lowercase-sha256>",
  "message": "Undeclared effect",
  "violation": {
    "observed": "Console::Write",
    "declared": []
  },
  "source": {
    "file": "src/example.wol",
    "start": 10,
    "end": 20
  },
  "repair": {
    "repairable": true,
    "classes": [
      "remove_operation",
      "modify_contract",
      "split_operation"
    ],
    "forbidden": [
      "suppress"
    ]
  },
  "impact": {
    "dependents": [],
    "tests": []
  }
}
```

## Rules

-   stable schema version
-   deterministic field meaning
-   stable diagnostic codes
-   no vague "something went wrong" errors
-   repair hints must never claim certainty when only heuristic

V1 anti-workaround diagnostics include these additive codes:

| Code | Severity | Meaning |
|---|---|---|
| WOLIO-ANALYZER-001 | error | semantic suppression or forbidden bypass |
| WOLIO-ANALYZER-002 | error | swallowed/unhandled error substitution |
| WOLIO-ANALYZER-003 | error | hidden dependency or effect pattern |
| WOLIO-ANALYZER-004 | error | configured forbidden business constant |
| WOLIO-ANALYZER-005 | warning | heuristic workaround or suspicious duplication |

Errors are policy/compiler rejection; warnings are findings and cannot be
presented as proof of business incorrectness. Repair classes must identify
valid structured alternatives and may not include `suppress` or `ignore`.

### Enforceable rule boundary

001 is emitted for mutation requests attempting diagnostic suppression or
verification bypass. 002 and 003 are descriptive aliases only: emit the existing
WOLIO-ERROR-001, WOLIO-DEPENDENCY-001 or WOLIO-EFFECT-001 for actual language
violations, never duplicate diagnostics. Exhaustive Result handling that returns
Ok from an Err arm is not inherently illegal; business correctness requires a
contract, test, or explicit policy.

004 is opt-in policy enforcement in M04. Manifest `[analysis]` may contain
`forbidden_int_literals = [integer, ...]` and
`forbidden_string_literals = [string, ...]`, default empty. Arrays must be unique
and correctly typed. Exact literal values in production implementation ASTs
(including folded unary-minus integer literals) are rejected when listed;
tests, contract predicates and intent are excluded. This is an explicit literal
ban, not a claim to detect business meaning. Policy is snapshot-bound and may
be changed only by owner-authorized workflow; no inline suppression exists.

005 is enabled by default at M04 for structurally identical normalized match-arm
bodies after bound-variable renaming. Emit one warning per match, with related
arm spans; equal handling may be legitimate and does not fail verify or change
approval tier. No general AI-debt/business-truth detection is promised. Analyzer
runs after successful static checks and before evidence aggregation; language
errors stop dependent analysis. Other heuristics belong to E06.
