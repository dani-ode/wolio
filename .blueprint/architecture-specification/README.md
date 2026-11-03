# Wolio Technical Specification

This document set is the implementation-level specification derived from
the Wolio blueprint.

The goal is no longer to describe what Wolio should be conceptually. The
goal is to define:

-   crate responsibilities
-   public interfaces
-   data ownership
-   dependency direction
-   invariants
-   error contracts
-   test requirements
-   implementation order
-   explicit non-goals for V1

## Source principles

The specification preserves the project decisions already established:

-   Static typing, `Result`/`Option`, exhaustive matching
-   Explicit dependencies and effects
-   Contracts and semantic IDs
-   Change scope and impact analysis
-   AI Agent API
-   Derived semantic graph
-   Architecture contracts as project policy
-   Compiler verification
-   AI analyzer separate from compiler legality
-   Source code is one representation; semantic AST is first-class
-   Rust implementation and LLVM backend
-   Minimal runtime
-   No filesystem-as-type-system
-   No universal mandatory two-tests-per-function rule
-   No line-number identity
-   No universal MAX_LOC compiler rule

The project must remain evidence-driven. Features in Tier 2/3 are not
allowed to silently enter the V1 core.

## V1 product boundary

V1 consists of two tightly integrated layers:

1.  Wolio Language Platform
2.  Wolio AI Construction System

The first coding milestone is not a complete language. It is a vertical
slice:

`source -> lexer -> parser -> AST -> semantic model -> diagnostics -> CLI`

Then:

`semantic model -> contracts -> effects -> dependency verification -> test verification`

Then:

`agent request -> inspect/plan -> structured mutation -> verify`

Only after this slice is stable should LLVM/native execution become the
next major target.

## Workspace

``` text
wolio/
├── Cargo.toml
├── crates/
│   ├── wolio-lexer/
│   ├── wolio-parser/
│   ├── wolio-ast/
│   ├── wolio-types/
│   ├── wolio-semantic/
│   ├── wolio-contracts/
│   ├── wolio-effects/
│   ├── wolio-resolver/
│   ├── wolio-analyzer/
│   ├── wolio-verify/
│   ├── wolio-ir/
│   ├── wolio-codegen/
│   ├── wolio-runtime/
│   ├── wolio-package/
│   ├── wolio-agent/
│   ├── wolio-cli/
│   ├── wolio-format/
│   └── wolio-lsp/
├── tests/
├── fixtures/
├── examples/
└── docs/
```

## Dependency rule

Lower-level representation crates must not depend on the CLI, agent, or
code generator.

The intended direction is:

``` text
lexer
  ↓
parser
  ↓
ast
  ↓
types
  ↓
semantic
  ↓
contracts / effects / resolver
  ↓
analyzer / verify
  ↓
ir
  ↓
codegen / runtime

package, agent, format, lsp, cli consume the semantic/compiler APIs.
```

## Implementation rule

Every crate specification defines:

-   purpose
-   owned concepts
-   public API
-   internal modules
-   invariants
-   errors
-   tests
-   non-goals
-   completion criteria

No crate is considered complete merely because it compiles.
