`.blueprint/01-architecture/DEPENDENCY_RULES.md`
# Crate Dependency Rules

## Objective

Keep the compiler modular enough that the AI-facing construction system
can consume semantic information without importing the backend.

## Allowed dependency graph

The historical diagram below is processing order, not Cargo dependency direction.
DELIVERY_POLICY.md defines actual `depends on` edges and the interpreter branch.
In particular runtime must not depend on codegen and semantic data must not depend
on its analysis passes.

``` text
wolio-lexer
    ↓
wolio-parser
    ↓
wolio-ast
    ↓
wolio-types
    ↓
wolio-semantic
    ├── wolio-contracts
    ├── wolio-effects
    └── wolio-resolver
          ↓
wolio-analyzer
    ↓
wolio-verify
    ↓
wolio-ir
    ↓
wolio-codegen
    ↓
wolio-runtime
```

Tooling:

``` text
wolio-package -> semantic/resolver/compiler interfaces
wolio-agent   -> semantic/contracts/effects/verify/analyzer
wolio-format  -> AST/source model
wolio-lsp     -> parser/semantic/diagnostics/agent-facing queries
wolio-cli     -> all public application services
```

## Hard rules

1.  `wolio-ast` must not depend on `wolio-semantic`.
2.  `wolio-semantic` must not depend on LLVM.
3.  `wolio-contracts` must not perform code generation.
4.  `wolio-analyzer` may report architecture/design concerns but must
    not redefine language legality.
5.  `wolio-verify` is the authoritative legality gate for verification.
6.  `wolio-agent` must not bypass verification.
7.  `wolio-cli` orchestrates; it does not implement compiler semantics.
8.  `.wolio/` data is derived and must be rebuildable.
9.  Semantic IDs must not depend on file paths or line numbers.
10. Project architecture rules are data/configuration, not hardcoded
    compiler policy.

## Cycle policy

Cargo dependency cycles are forbidden.

Semantic cycles in the language are not automatically forbidden. Whether
a graph cycle is legal is a semantic rule, not a Rust crate dependency
rule.

## Public API policy

Each crate exposes a small stable API through `lib.rs`. Internal
implementation modules remain private unless another crate genuinely
needs the abstraction.
