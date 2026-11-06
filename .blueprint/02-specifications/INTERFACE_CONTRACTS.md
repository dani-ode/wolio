`.blueprint/02-specifications/INTERFACE_CONTRACTS.md`
# Shared interface contracts V1

Normative clarification for revision 1.2. These rules connect the language,
package, CLI and tooling specifications; internal Rust function signatures remain
implementation choices.

## Project inputs

Project root contains `wolio.toml`, `src/**/*.wol`, optional `tests/**/*.wol`,
`wolio.ids.json`, and (with external dependencies) `wolio.lock`. Source discovery
uses sorted normalized root-relative paths, does not follow directory symlinks,
and excludes `.wolio/`, `.git/`, build output and dependency caches. Duplicate
module names in the compiled package fail. Test modules enter test/verify only.

```toml
[package]
name = "example"
version = "0.1.0"
[language]
version = "1"
[entrypoint]
module = "main"
operation = "main"
```

Libraries may omit entrypoint; run/build require it. Entry operations must be
public for package invocation. `init <directory>` creates this manifest, an
identity map and a valid `src/main.wol` returning Unit, and refuses to overwrite
existing files. No implicit directory traversal outside the chosen target.

## CLI and output

All commands accept `--json`; project commands accept `--project <directory>`
(default current directory). `check [file]` additionally accepts one standalone
source for frontend diagnostics; a project is required for imports and execution.
Command availability: M01 init/check; M02 run; M03 test/verify/inspect/impact;
M04 agent; M05 build; M07 add/remove/install/update/list/info/graph/fmt/lsp.
Unsupported commands/features produce a usage or FEATURE diagnostic, never a
successful no-op. M01 check reports unimplemented semantic obligations not_run.

JSON command envelopes follow EVIDENCE_MODEL: status is `success` or `failure`,
diagnostics/evidence are arrays, and command-specific data is in `result`.
`run` additionally has `output` (captured console string) and `result.value`
(integer or null for Unit). Human run prints console output followed by the Int
result and newline, if any. Exit status follows EVIDENCE_MODEL, not returned Int.
JSON stdout is exactly one object plus newline; logging is stderr. Agent and LSP
use their respective streaming protocols instead of this single-object envelope.

`inspect [symbol-id]` returns sorted nodes/edges and snapshot; no ID means the
whole graph. `impact --before <project-dir> --after <project-dir>` compares two
explicit snapshots, returns semantic changes, compatibility and reverse reachable
callers/tests with edge paths. Neither command writes inputs. `test` runs tests in
stable module/name order with isolated captured output; verify also applies the
profile's evidence gates. `build` checks before lowering. `fmt --check` is read-only.

## Names and dependency packages

V1 retains flat module imports. At M07 the locked package closure exports public
modules into the import lookup; colliding module names across that closure are
rejected with both package identities. There is no implicit package alias syntax.
Imports cannot access private declarations. Dependency packages are read-only
inputs; structured mutations may change root sources/manifest/lock only within
authorized scope. Dependencies do not supply entrypoints to the root project.

## Canonical hashes and identities

Canonical JSON uses UTF-8, sorted object keys, no insignificant whitespace,
no floats, and preserves array order unless the schema specifies sorting. Hash
inputs are domain-tagged and encoded as a sequence of length-prefixed byte fields
(unsigned 64-bit big-endian lengths), eliminating concatenation ambiguity.
Snapshot fields are sorted relative paths plus bytes for source and explicit
metadata/policy files; absent optional files have an explicit absent marker.
SHA-256 is lowercase hex. Contract normalization ignores comments/spans and
formatting, preserves expression/statement order, resolves names to IDs, and
sorts set-valued clauses only. It never simplifies algebra or reorders predicates.

Identity-map schema: schema_version, package_id (UUIDv4), declarations array
sorted by module/kind/name with id/module/kind/name, and tombstones array of IDs.
Unregistered declarations receive ephemeral IDs derived from snapshot plus
module/kind/name. `init` persists initial IDs; structured creation persists new
UUIDs. `inspect --register-ids` is the explicit mutating reconciliation command:
it allocates missing IDs, tombstones removals, preserves exact mappings, and
rejects ambiguous rename until the caller supplies an explicit old/new mapping.
It never heuristically transfers identities. This option is available from M02.

## Lexical and binding clarifications

Keywords are all quoted alphabetic grammar terminals, reserved in every position.
Builtin type/constructor names and console cannot be redeclared. `result` is
reserved as a binding name. Parameters, lets and pattern payloads may not shadow
visible bindings; duplicate fields, variants and declarations fail. Imports may
not form cycles. Operations have exactly one implementation in the same module.
Strings reject unescaped control characters and unpaired surrogate escapes;
JSON surrogate pairs decode to one Unicode scalar. CRLF and LF are whitespace;
spans count original UTF-8 bytes. No trailing commas or block comments in V1.
The grammar's unary prefix applies after field selection (`-x.value`). Int MIN
remainder -1 is zero; division MIN/-1 faults. A Result-valued binding is handled
only by exhaustive match or forwarding it as a returned expression; assigning,
comparing, or merely mentioning it does not discharge handling obligations.
