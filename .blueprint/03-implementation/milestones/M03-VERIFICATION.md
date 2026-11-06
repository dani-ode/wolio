`.blueprint/03-implementation/milestones/M03-VERIFICATION.md`
# Milestone 3 --- Verification

> Revision 1.1: runnable trial, contracts/tests/effects/dependencies/impact and
> pilot. Predicates are runtime-guarded, not universally proved. Scope/capability
> moves to M04; architecture policy to extensions. DELIVERY_PLAN controls scope.

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
