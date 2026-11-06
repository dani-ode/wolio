# Diagnostic Schema V1

> Superseded example: DIAGNOSTIC_SPEC.md defines the canonical nested `repair`
> shape and lowercase category; EVIDENCE_MODEL.md resolves spans, code registry,
> evidence and CLI envelope. Do not implement the flat draft example below.

**Status:** `Proposed`  
**Supersedes:** none; extends `DIAGNOSTIC_SPEC.md`.

Every diagnostic is machine-readable and deterministic:

```json
{
  "schema_version": 1,
  "code": "WOLIO-TYPE-001",
  "severity": "error",
  "category": "TYPE",
  "target": "semantic_id",
  "message": "...",
  "expected": "...",
  "observed": "...",
  "source": {"file": "...", "start": 0, "end": 0},
  "repairable": true,
  "repair_classes": [],
  "forbidden_repairs": ["suppress"],
  "impact": {"dependents": [], "tests": []}
}
```

Codes, severity, category, target, and field meanings are stable within schema
version 1. Heuristic repair hints must be labelled as suggestions, never facts.

`OPEN`: complete category/code registry and source span encoding.
