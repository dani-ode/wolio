# Delivery and toolchain policy

Rust/Cargo implements the compiler; Linux x86_64 is the V1 supported platform.
Other platforms are best-effort until added with CI. T001 selects the then-current
stable Rust release, records its exact version in rust-toolchain.toml, commits
Cargo.lock, and uses GitHub Actions on a pinned runner image. No floating version
is permitted in recorded release evidence. M05 similarly pins a supported LLVM
major and exact build/container digest before backend implementation. These are
execution inputs, not unresolved language decisions.

Interpreter first: typed semantic expressions execute sequentially with checked
arithmetic and contract guards. Host panic is an internal compiler failure, not
a language Result. M05 lowers the same checked semantics into typed SSA-like IR
with explicit blocks, branches, calls, constructors, tags, arithmetic guards, and
contract guards. IR verifier rejects ill-typed edges and missing terminators.
LLVM optimization must preserve faults/guards; differential fixtures are the
release gate. Immutable heap values use reference counting initially; no exposed
pointer identity or mandatory tracing GC. Console is the sole host adapter.

The CLI composes frontend, semantic checks, interpreter/backend, and tooling.
Scope authorization belongs to agent application, not ordinary source legality.
Crate notes describe eventual module boundaries; create a crate only when useful.
Dependency arrows mean "depends on": parser -> lexer/AST; resolver -> AST/semantic;
type/contracts/effects passes -> semantic; interpreter -> semantic/runtime;
codegen -> IR; agent -> verification/query services. Runtime never depends on
codegen or LLVM. No dependency cycles. Diagnostic values are shared low-level data.

## Manifest and versions

Use `[package] name/version`, `[language] version = "1"`, and `[entrypoint]
module/operation`. `[module] path` is required only for published Git packages.
This supersedes earlier `[project]` manifest examples. Compiler, language,
diagnostic schema, protocol, and package versions are independent. Unsupported
major versions fail explicitly. V1 source-breaking changes require a new language
major; additive diagnostic fields do not. Publish migration notes with changes.

Release artifacts: Linux x86_64 tar.gz, SHA-256 checksums, exact toolchain/lock
metadata, license and release notes. `install --locked --offline` never resolves
new versions or accesses the network. Failed builds never reuse stale success
evidence. Reproducible semantic outputs are mandatory; bit-identical native
binaries are measured separately with environment recorded.

## M07 services

Formatter parses and prints canonical syntax, preserves comments, uses four-space
indentation and LF, is idempotent, and preserves semantic AST/IDs. `fmt --check`
does not write. LSP supports initialize/shutdown, incremental document updates,
diagnostics, hover, definition, references, and rename using the same snapshot
model. Unsaved buffers never overwrite disk. Rename emits a reviewed workspace
edit including identity mappings; stale document versions reject application.
Package rules remain in PACKAGE_MANAGER_SPEC.md with the additional lock rules
specified there. No package install scripts or implicit capability grants.
