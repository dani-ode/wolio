# Milestone 2 --- Semantic Core

## Goal

Make Wolio meaning machine-queryable.

Implement:

-   semantic repository
-   stable IDs
-   symbol index
-   operation index
-   relation graph
-   contract representation
-   type system
-   resolver

## Acceptance

The CLI must be able to answer:

``` text
inspect operation
find dependencies
find dependents
find callers
find tests
find invariants
impact
```

without requiring the AI to scrape source text.
