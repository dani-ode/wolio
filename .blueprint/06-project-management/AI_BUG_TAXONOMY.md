# AI Failure Taxonomy

**Status:** `Proposed`

| ID | Failure | Example to benchmark |
|---|---|---|
| W01 | Wrong type | use `UserId` where `OrderId` is required |
| W02 | Missing error handling | discard a failed database result |
| W03 | Hidden dependency | read global configuration without declaration |
| W04 | Wrong dependency direction | domain imports infrastructure |
| W05 | Missing test | add a payment path without boundary coverage |
| W06 | Incorrect edge case | empty input accepted as valid identifier |
| W07 | Wrong algorithm | use linear search where ordering is required |
| W08 | Wrong business rule | apply discount to an ineligible customer |
| W09 | Race condition | lost update in concurrent balance writes |
| W10 | State inconsistency | mark order paid before transaction commits |
| W11 | Security flaw | authorize using user-supplied role |
| W12 | Contract misunderstanding | weaken a precondition silently |
| W13 | Incorrect assumption | assume an external API is idempotent |
| W14 | Unintended side effect | logging operation mutates persistent state |
| W15 | Regression | unrelated endpoint changes after refactor |

Each benchmark case must record the failure ID, prompt, outcome, iterations,
tokens, time, regressions, and security impact.
