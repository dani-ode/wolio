# Diagnostic Schema

Diagnostics are part of the AI interface, not just compiler text.

## Canonical shape

``` json
{
  "schema_version": 1,
  "code": "WOLIO-EFFECT-001",
  "severity": "error",
  "category": "effect",
  "target": "fn_01J...",
  "message": "Undeclared effect",
  "violation": {
    "observed": "Database.Write",
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
