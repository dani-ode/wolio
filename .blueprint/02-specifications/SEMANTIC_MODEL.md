# Semantic model V1

The shared model contains packages, modules, nominal/record/enum types, operations,
implementations, clauses, effects, tests, and source locations. Edges are imports,
calls, depends_on, returns, uses_type, has_contract, has_effect, and tests.
Impact is reverse reachability over typed edges; explain the edge path rather
than claiming that reachability proves behavioral breakage.

## Stable identity

Persistent IDs are UUIDv4 strings prefixed `sym_`, allocated by init/import tooling
and stored in version-controlled `wolio.ids.json` with schema_version 1, package
UUID, and declaration mappings (module, kind, name, ID). This explicit metadata
is authoritative; `.wolio/` is only a derived cache. Formatting and filesystem
moves preserving module declarations do not change IDs. Structured rename updates
the mapping atomically. Raw ambiguous renames require explicit reconciliation,
never heuristic identity transfer. Duplicate IDs or mappings fail checking.

Read-only check may use deterministic temporary identities for unregistered
declarations, labelled `ephemeral`; finalization requires persisted IDs. No check
silently writes metadata. Deletion tombstones IDs; recreation receives a new ID.
Contract clauses use operation ID plus normalized clause hash and occurrence
index. Changes produce new clause identities, retaining old/new links in diffs.

Types are interned semantic nodes, not text aliases. Builtin type applications
include their arguments in identity; nominal identity uses declaration ID.
There is no ownership/concurrency model in immutable, sequential V1.

## Determinism and provenance

Queries sort IDs and edges, and carry a source snapshot hash. Snapshots hash
sorted relative source paths and bytes, manifest, identity map, lockfile, and
policy; generated cache files are excluded. Hash algorithm is SHA-256.
Change provenance stores intent, actor, base/result hashes, approved diff,
affected IDs, and evidence references. Value provenance is an extension,
distinct from change provenance. Human intent is not treated as a proven fact.
