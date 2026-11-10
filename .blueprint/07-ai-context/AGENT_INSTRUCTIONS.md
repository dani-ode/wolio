`.blueprint/07-ai-context/AGENT_INSTRUCTIONS.md`
# AI agent instructions

Start with DOCUMENT_AUTHORITY.md, BASELINE-V1.md and `.execution/current-state.md`.
Find the assigned ID in 04-tasks/task-index.md, read its dedicated task file and
04-tasks/EXECUTION_CONTRACTS.md. Read normative behavior and acceptance families for that task.
Supporting architecture/crate sketches cannot add syntax, effects or release gates.

Before edits, check actual readiness and existing user changes. Implement the
bounded task, run its checks, then record results under `.execution/`. If baseline
semantics conflict or are absent, document the exact blocker and seek amendment.
Do not equate documentation completion, owner acceptance or runtime tests with a
working compiler, passing release or formal proof.
