# wolio-analyzer

## Responsibility

Detect code that is technically legal but suspicious, risky, or likely
to increase AI maintenance debt.

The analyzer is advisory unless a project policy promotes a rule to a
hard verification rule.

## Initial rules

``` text
AI001 suspicious fallback
AI002 hardcoded business value
AI003 excessive complexity
AI004 high dependency fanout
AI005 implicit behavior
AI006 low contract coverage
AI007 low test diversity
AI008 suspicious global state
AI009 architecture smell
AI010 high change blast radius
```

## AI debt metrics

``` text
Contract Coverage
Test Coverage
Dependency Explicitness
Effect Explicitness
Mutation Complexity
Branch Complexity
Change Blast Radius
Semantic Ambiguity
Fallback Count
Global State Count
Unsafe Count
AI Context Complexity
```

## Critical distinction

Compiler:

> This is illegal.

Analyzer:

> This may be wrong or dangerous.

AI:

> Decide whether a repair or architectural change is required.

## Tests

-   known-good fixtures
-   known-bad fixtures
-   false-positive regression suite
-   rule stability
