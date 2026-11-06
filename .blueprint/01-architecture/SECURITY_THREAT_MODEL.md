`.blueprint/01-architecture/SECURITY_THREAT_MODEL.md`
# Security Threat Model V1

**Status:** `Proposed`

| Threat | Minimum control |
|---|---|
| secret leakage | semantic secret type, redaction, no source literals |
| authorization bypass | explicit capability checks and deny-by-default |
| capability escalation | narrow non-transitive grants and audit log |
| malicious dependency/supply chain | lockfile, provenance, review, verification |
| unsafe FFI | explicit capability, owner, reason, contract, tests |
| network exfiltration/data exposure | declared effects, scope, review, redaction |
| prompt-induced scope expansion | immutable allow/deny scope and approval |
| workaround persistence | analyzer rules and regression fixtures |

No operation finalizes without verification. Security-boundary changes require
human review. `OPEN`: dependency signing policy, sandbox boundary, and key
management integration.
