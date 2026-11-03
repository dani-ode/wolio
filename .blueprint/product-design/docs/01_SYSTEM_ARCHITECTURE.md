# Wolio System Architecture

## 1. Workspace architecture

Use a Rust Cargo workspace.

``` text
wolio/
├── crates/
│   ├── wolio-lexer/
│   ├── wolio-parser/
│   ├── wolio-ast/
│   ├── wolio-semantic/
│   ├── wolio-types/
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
│   ├── compiler/
│   ├── semantic/
│   ├── contracts/
│   ├── agent/
│   ├── security/
│   └── benchmarks/
├── examples/
├── docs/
├── fixtures/
├── Cargo.toml
└── README.md
```

Do not create every crate as a stub on day one. Create the workspace
boundaries first, then implement them in dependency order.

## 2. Compiler pipeline

``` text
source files
    ↓
lexer
    ↓
parser
    ↓
AST
    ↓
name resolution
    ↓
semantic model
    ↓
type checking
    ↓
contract checking
    ↓
effect checking
    ↓
dependency checking
    ↓
capability/scope checking
    ↓
semantic analysis
    ↓
Wolio IR
    ↓
LLVM IR
    ↓
native executable
```

The semantic model must be reusable by the AI tooling. Do not build a
second incompatible representation for the agent.

## 3. Source of truth

The source files and explicit project artifacts are authoritative.

Generated structures are derived:

``` text
.wolio/
├── ast/
├── semantic/
├── symbols/
├── graph/
├── contracts/
├── tests/
├── decisions/
└── cache/
```

Never make generated graph files the only source of truth.

The graph must be rebuildable from source and project metadata.

## 4. Semantic model

Every declaration receives a stable semantic ID.

Example:

``` text
fn CreatePayment
id = fn_01J...
```

The ID should survive ordinary file moves and formatting changes.

Semantic entities include:

-   module;
-   package;
-   entity;
-   value type;
-   semantic type;
-   operation;
-   implementation;
-   contract;
-   error;
-   effect;
-   capability;
-   test;
-   invariant;
-   decision;
-   endpoint;
-   event;
-   resource.

## 5. Semantic relationships

Do not model only:

``` text
A imports B
```

Model:

``` text
requires
consumes
produces
transforms
depends_on
affects
invalidates
authorizes
persists
emits
reads
writes
```

Example:

``` text
ProductUnitPrice
    ↓
CalculateOrderSubtotal
    ↓
OrderSubtotal
    ↓
CreateOrder
    ↓
PaymentIntent
```

This allows impact analysis to reason about meaning rather than only
text references.

## 6. Compiler vs analyzer

### Compiler MUST reject

-   syntax errors;
-   type errors;
-   unresolved symbols;
-   undeclared dependency;
-   undeclared effect;
-   invalid contract implementation;
-   illegal capability;
-   invalid secret literal;
-   illegal fallback;
-   non-exhaustive match;
-   semantic suppression;
-   invalid scope mutation.

### Analyzer SHOULD detect

-   suspicious hardcoded business values;
-   workaround patterns;
-   effect substitution;
-   semantic mismatch;
-   architecture degradation;
-   excessive complexity;
-   unnecessary dependency;
-   suspicious test duplication;
-   low semantic coverage;
-   high blast radius.

### AI MAY propose

-   implementation repair;
-   contract change;
-   dependency addition;
-   refactoring;
-   test addition;
-   architecture change;
-   capability escalation.

### Human MUST approve

-   contract changes;
-   security boundary changes;
-   capability escalation;
-   architecture invariant changes;
-   persistent schema changes;
-   unsafe exceptions.

## 7. Backend

Use:

``` text
Wolio source
    ↓
Wolio IR
    ↓
LLVM IR
    ↓
LLVM
    ↓
native binary
```

Do not implement a CPU backend.

The compiler team's differentiator is semantics and verification, not
instruction selection.

## 8. Runtime

Keep runtime small.

Prefer:

-   native execution;
-   no mandatory GC;
-   no mandatory VM;
-   no reflection by default;
-   explicit runtime capabilities.

Concurrency should eventually compile to a runtime abstraction, but
language semantics must remain independent of a specific scheduler.
