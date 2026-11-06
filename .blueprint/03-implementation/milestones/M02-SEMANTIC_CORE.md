`.blueprint/03-implementation/milestones/M02-SEMANTIC_CORE.md`
# Milestone 2 --- Semantic Core

> Revision 1.1 adds interpreter/run, nominal/record/enum types, Result/Option
> and exhaustive matching. Contract predicates are unsupported until M03.
> DELIVERY_PLAN controls acceptance.

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
