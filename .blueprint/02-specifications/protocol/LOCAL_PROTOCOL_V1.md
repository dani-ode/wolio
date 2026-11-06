`.blueprint/02-specifications/protocol/LOCAL_PROTOCOL_V1.md`
# Local agent protocol V1

M04 provides local JSON-lines over stdin/stdout; no HTTP server, remote auth,
or network transport is included. The process runs under the invoking OS user.
This protocol constrains managed changes, not an agent with independent shell
access. Untrusted code execution needs an OS sandbox; metadata is not a sandbox.

Request envelope: `protocol_version: 1`, unique string `request_id`, `method`,
`base_snapshot`, and object `params`. Response repeats version/request_id and
contains exactly one of `result` or `error` (code, message, diagnostics).
Methods: inspect, impact, plan, apply, verify, finalize. Unknown versions/methods
fail before mutation. Queries require the snapshot they describe.

Plan params contain intent, operation array, allowed symbol/path scope, and
requested capabilities. Operations are create_declaration, replace_implementation,
replace_contract, rename_symbol, move_symbol, add_test, delete_symbol. Each carries
target ID (except create), expected old hash, and typed payload or source fragment
parsed into the V1 AST. Parse/type checks still apply to structured mutations.

Plans return a plan ID, canonical diff hash, affected IDs, compatibility classes,
required approvals, and diagnostics. Contract/dependency/effect/security changes
require owner approval. Ordinary implementation/test edits inside scope may be
automatic. Deny overrides allow; default is deny. Scope matches normalized
project-relative paths and IDs; symlink escape and `..` traversal are rejected.

Approval is a local owner-authored record outside agent-writable scope: approver,
plan hash, base snapshot, capabilities, UTC expiry, and revocation state. It is
valid for one application only, is not delegable, and expires after 24 hours at
most. The project owner is the invoking repository maintainer; remote identity
federation is outside V1. Agent cannot write its own approval records.

Apply stages changes in an isolated temporary workspace, validates scope both
before and after, checks the current snapshot, runs verification, and acquires
an exclusive project mutation lock before compare-and-swap publication. Changed
base or approval returns WOLIO-STALE-001. A journal records old/new content hashes;
crash recovery completes or restores the entire managed change before another
mutation. Never discard unrelated user edits. External side effects are not
rolled back by source rollback. Tests receive only explicitly granted execution
capabilities. Finalize binds successful evidence to the published snapshot.

Repeated request IDs with identical payload replay the recorded result; changed
payload with the same ID fails. Records are append-only JSONL under `.wolio-log/`
(explicit project artifacts, not rebuildable cache). Secret values are excluded.
Repair is another plan with the same constraints; default budget is 3 attempts,
then report unresolved diagnostics. No suppression or automatic contract weakening.

Acceptance: valid edit, stale edit, missing/expired/revoked approval, traversal,
symlink escape, denied scope, failed verification, crash recovery, duplicate
request, changed duplicate payload, and preservation of concurrent user edits.
