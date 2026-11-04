# wolio-format

## Responsibility

Canonical source formatting.

## Requirements

-   formatting must not change semantic identity
-   formatter must be deterministic
-   formatter must never silently modify semantics
-   comments/documentation preservation policy must be explicit

## API

``` rust
pub fn format(source: &str) -> Result<String, FormatError>;
```

## Tests

-   idempotence
-   AST equivalence before/after
-   comments
-   multiline constructs
