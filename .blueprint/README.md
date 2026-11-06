# Wolio Blueprint

This directory is the authoritative documentation and engineering specification for Wolio.

## Document authority

0. [`BASELINE-V1.md`](BASELINE-V1.md) defines revision 1.1 scope, normative
   specifications, and overrides earlier conflicting aspirational examples.
1. `blueprint.md` defines the long-term vision and non-negotiable principles.
2. `01-architecture/` and `02-specifications/` define approved technical behavior.
3. `03-implementation/` defines the current implementation baseline.
4. `04-tasks/` contains executable engineering work.
5. `05-quality/` defines acceptance and release standards.
6. `07-ai-context/` defines how coding agents must use this repository.
7. `99-references/` contains supporting references that are not authoritative.

When documents conflict, the higher-level document wins. Proposed changes must be recorded before implementation.

## Status values

Documents use one of: `Draft`, `Proposed`, `Approved`, `Implemented`, `Deprecated`, or `Superseded`.

## Fast path for coding agents

Start with [`MAP.md`](MAP.md). It is the helicopter view and contains the
current phase, dependency order, and the smallest set of documents to read
for each kind of change. Then open the relevant task in `04-tasks/`.

The normal loop is: choose a `READY` task, read its linked authority documents,
inspect only its affected modules, implement the smallest scoped change, run
verification, and record evidence. Do not read the whole blueprint by default.
If a decision is missing, mark it `OPEN` instead of inventing behavior.

Progress and evidence, including baseline preparation, are recorded in the
repository-level `.execution/` ledger. Blueprint status is not implementation
status. Revision 1.1 is a freeze candidate; the owner's acceptance record is
separate from the author's completion report.
