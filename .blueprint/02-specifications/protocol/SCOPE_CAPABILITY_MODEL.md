`.blueprint/02-specifications/protocol/SCOPE_CAPABILITY_MODEL.md`
# Scope and Capability Model V1

> HISTORICAL / NON-NORMATIVE. LOCAL_PROTOCOL_V1.md and APPROVAL_MATRIX_V1.md
> define the accepted V1 behavior.

> LOCAL_PROTOCOL_V1.md defines V1 owner identity, approval expiry/revocation,
> nondelegation, scope matching and atomic mutation. Earlier OPEN items below
> are superseded. Effect declarations describe behavior, not execution authority.

**Status:** historical/non-normative; superseded by LOCAL_PROTOCOL_V1.md and APPROVAL_MATRIX_V1.md

Scopes define `READ`, `WRITE`, `CREATE`, `DELETE`, and `EXECUTE` over semantic
entities and derived paths. Capabilities are narrow authorities such as
`Network.Access`, `Secret.Read`, `Schema.Migrate`, and `Unsafe.Execute`.
Default deny applies to omitted scope/capability. Out-of-scope mutation is a
hard failure and cannot be repaired by suppression.

| Change | Policy |
|---|---|
| implementation within approved scope | AUTO |
| tests and ordinary inspection | AUTO |
| dependency/effect addition | REVIEW |
| contract/security-boundary change | REVIEW |
| secret access, migration, unsafe execution | REVIEW or BLOCK |
| scope expansion | REVIEW |

`OPEN`: approval identity, expiry/revocation, and capability delegation format.
