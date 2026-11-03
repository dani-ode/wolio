# Milestone 3 --- Verification

## Goal

Make the compiler enforce the core anti-error rules.

Implement:

-   static typing
-   Result/Option
-   exhaustive match
-   explicit dependencies
-   explicit effects
-   contracts
-   architecture policy
-   capability checks
-   structured diagnostics

## Acceptance

These must be rejected:

1.  undeclared dependency
2.  undeclared effect
3.  type mismatch
4.  contract violation
5.  forbidden architecture edge
6.  missing exhaustive branch
7.  unauthorized capability
8.  out-of-scope mutation
