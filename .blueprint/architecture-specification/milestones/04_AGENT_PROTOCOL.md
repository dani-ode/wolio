# Milestone 4 --- AI Construction Protocol

## Goal

Allow an AI agent to modify a project without arbitrary file editing.

## Implement

-   inspect API
-   plan API
-   structured mutation model
-   scope
-   capability
-   authorization
-   semantic diff
-   verification
-   rollback

## Acceptance

An agent can:

``` text
inspect → plan → apply → verify
```

for a small feature.

An agent cannot:

-   modify outside scope
-   add undeclared dependency
-   add undeclared effect
-   weaken a contract without authorization
-   bypass verification
