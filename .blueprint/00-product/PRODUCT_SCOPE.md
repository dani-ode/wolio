`.blueprint/00-product/PRODUCT_SCOPE.md`
# Wolio Product Scope — V1

> Revision 1.1 supersedes the two-product MVP below: Wolio's primary product is
> the language. BASELINE-V1.md defines M01–M07 and the M03 trial stop point.
> Linux x86_64 is supported; the repository owner is approval authority;
> measurable acceptance and benchmark thresholds are in DELIVERY_PLAN.md.
> The earlier OPEN list is historical, resolved by those decisions.

**Status:** `Proposed`  
**Decision authority:** human project owner for scope and contract changes.

## Product

Wolio is both (1) a statically typed programming language/toolchain and (2) an
AI construction system that turns intent into bounded, structured, verifiable
changes. The AI construction system is the differentiator; the language is its
verification substrate.

## V1 outcome

V1 must demonstrate a deterministic compiler vertical slice and an auditable
agent workflow: inspect, propose, authorize, apply structured changes, and
verify. The minimum language surface is the M01 slice plus the Tier 0 concepts
listed in `blueprint.md`; features not implementable in M01 remain specified,
not falsely claimed as implemented.

## In scope

- stable language/diagnostic versioning;
- modules, declarations, deterministic parsing, AST, semantic IDs;
- explicit contracts, errors, dependencies, and effects;
- structured diagnostics and verification gates;
- bounded scopes, capabilities, semantic diffs, and provenance;
- benchmark evidence against conventional-language baselines.

## Out of scope for M01

LLVM/native backend, package registry, LSP, distributed execution, advanced
formal verification, unrestricted text mutation, and production-grade
concurrency are deferred to later milestones.

## OPEN decisions

- `OPEN`: exact supported OS/architecture matrix;
- `OPEN`: final human approver identity and escalation process;
- `OPEN`: quantitative definition of “safe enough” and V1 benchmark threshold.
