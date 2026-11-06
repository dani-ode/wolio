`.blueprint/DOCUMENT_AUTHORITY.md`
# Document authority — revision 1.3

This register is the entry point for the entire blueprint. Revision 1.3 is a
pre-implementation freeze candidate, not owner acceptance or shipped software.

## Binding reading order

1. This register and `BASELINE-V1.md`: scope, authority and amendment policy.
2. `00-product/PROJECT_COVERAGE.md`: complete product inventory and release boundaries.
3. Normative behavior: `02-specifications/language/CORE_V1.md`,
   `02-specifications/CONTRACT_DESIGN.md`, `02-specifications/SEMANTIC_MODEL.md`,
   `02-specifications/INTERFACE_CONTRACTS.md`,
   `02-specifications/protocol/LOCAL_PROTOCOL_V1.md`,
   `02-specifications/protocol/MUTATION_SCHEMA_V1.md`,
   `02-specifications/protocol/APPROVAL_MATRIX_V1.md`,
   `02-specifications/package-manager/PACKAGE_MANAGER_SPEC.md`,
   `02-specifications/verification/DIAGNOSTIC_SPEC.md`,
   `02-specifications/verification/EVIDENCE_MODEL.md`.
4. Delivery: `03-implementation/IMPLEMENTATION_SPECIFICATION_V1.md`,
   `03-implementation/DELIVERY_POLICY.md`, `06-project-management/DELIVERY_PLAN.md`,
   `05-quality/ACCEPTANCE_MATRIX.md`, `04-tasks/EXECUTION_CONTRACTS.md`.
5. Task files and engineering guidance implement those rules, never extend them.

Revision 1.3 incorporates ADR-007 and supersedes conflicting revision 1.2
identity, mutation, approval, native-backend, and analyzer wording. Execution
baseline acceptance remains a separate owner decision before T001.

`02-specifications/EXTENSION_BOUNDARIES.md` and
`06-project-management/POST_V1_PLAN.md` are binding scope boundaries and research
gates, not accepted future syntax. V1 is the complete committed delivery scope.
Future releases require separate accepted baselines; there is no implicit V2.

## Supporting and historical material

Files not listed above are supporting rationale unless explicitly delegated by a
normative document. In particular old LANGUAGE_SPEC/LANGUAGE_DESIGN, WTP_SPEC,
AGENT_PROTOCOL_DESIGN, SCOPE_CAPABILITY_MODEL, crate API sketches, roadmap,
M00 reports, and broad security/architecture inventories cannot add V1 behavior.
Their examples, OPEN markers and approval labels describe their original revision.
`blueprint.md` is the long-term vision, not executable grammar. Supporting files
may inform implementation internals only when consistent with normative behavior.

Where two normative documents disagree, stop the affected task, record the exact
conflict and resolve it by amendment; do not choose whichever is convenient.
Execution facts are authoritative only in `.execution/`; document statuses never
claim code exists. A proposed design becomes frozen only through owner acceptance
identifying a committed revision. This audit does not supply that acceptance.
