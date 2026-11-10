`.blueprint/04-tasks/T004-diagnostics-cli.md`
# T004: Add deterministic diagnostics and CLI check

| Field | Value |
|---|---|
| Milestone | `M01` |
| Priority | `P0` |
| Depends on | T003 |
| Blocks | T020 (M02 entry) |
| Acceptance families | FRONT (CLI cases); completes M01 |
| Bounded areas | CLI crate (`wolio-cli`), shared diagnostic values, project discovery |
| Execution status | `.execution/task-status.md` (sole source) |
| Report | `.execution/reports/T004.md` |

## Objective
Expose parsing through the CLI and return machine-readable diagnostics for
valid and invalid source files.

## Authority
- Normative: [INTERFACE_CONTRACTS.md](../02-specifications/INTERFACE_CONTRACTS.md) (project inputs, CLI and output), [EVIDENCE_MODEL.md](../02-specifications/verification/EVIDENCE_MODEL.md), [DIAGNOSTIC_SPEC.md](../02-specifications/verification/DIAGNOSTIC_SPEC.md), [CORE_V1.md](../02-specifications/language/CORE_V1.md) (M01 profile)
- Acceptance: ACCEPTANCE_MATRIX `FRONT`, M01 row of DELIVERY_PLAN, DEFINITION_OF_DONE
- Supporting only: [SYSTEM_ARCHITECTURE.md](../01-architecture/SYSTEM_ARCHITECTURE.md), [WOLIO-CLI.md](../03-implementation/crates/WOLIO-CLI.md)

## Scope
In scope: `init` and `check` commands, project source discovery, JSON and human
diagnostic output, exit codes.

Out of scope (owner):
- Module uniqueness, imports and resolution (T020); identity targets (T023);
  `run` (T024); test/verify/inspect/impact (T032–T033).

## Normative requirements
- Command availability at M01: `init`, `check`. All commands accept `--json`;
  project commands accept `--project <directory>`; `check [file]` also accepts one
  standalone source. Unsupported commands/features produce a usage or
  `WOLIO-FEATURE-001` diagnostic, never a successful no-op (INTERFACE_CONTRACTS).
- `init <directory>` creates the manifest and a valid `src/main.wol` returning
  Unit, refuses to overwrite existing files and never traverses outside the
  target (INTERFACE_CONTRACTS).
- Discovery: `wolio.toml`, `src/**/*.wol`, optional `tests/**/*.wol`; sorted
  normalized root-relative paths; no directory symlink following; excludes
  `.wolio/`, `.git/`, build output and dependency caches (INTERFACE_CONTRACTS).
- JSON stdout is exactly one object plus newline with `schema_version`,
  `command`, `status` (`success`/`failure`), `diagnostics`, `evidence` and
  `result`; logging goes to stderr (EVIDENCE_MODEL, INTERFACE_CONTRACTS).
- Diagnostics follow the DIAGNOSTIC_SPEC shape, sort by path, start, code,
  target; `target` is null before identity assignment (EVIDENCE_MODEL).
- Exit codes: 0 success, 1 source rejection, 2 usage/configuration, 3 internal
  failure (EVIDENCE_MODEL).
- M01 check reports unimplemented semantic obligations `not_run` (CORE_V1).

## Acceptance criteria
- [ ] `wolio check` succeeds for valid input
- [ ] Invalid input has stable error code, span, and message
- [ ] CLI exit status distinguishes success and failure
- [ ] init creates a valid project without overwrites; check supports JSON
- [ ] Semantic checks not implemented at M01 are reported not_run
- [ ] Unsupported commands return a usage or FEATURE diagnostic, not success
- [ ] Project discovery order and exclusions are covered by fixtures
- [ ] All M01 DELIVERY_PLAN checks pass; common gates pass; report written

## Verification
```bash
cargo test --workspace
cargo run -p wolio-cli -- check tests/conformance/front/valid/empty.wol --json
cargo fmt --all -- --check
cargo clippy --workspace --all-targets -- -D warnings
```
Evidence: `.execution/reports/T004.md`

## Dependencies and open items
- Depends on: T003; blocks: T020 and therefore M02.
- Open: none.
