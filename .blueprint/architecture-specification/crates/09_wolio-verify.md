# wolio-verify

## Responsibility

Provide the authoritative verification pipeline.

## Verification order

``` text
parse
→ resolve
→ type check
→ contract check
→ effect check
→ dependency check
→ capability/scope check
→ test policy check
→ architecture policy check
→ semantic legality
```

## Result

``` rust
pub struct VerificationReport {
    pub status: VerificationStatus,
    pub diagnostics: Vec<Diagnostic>,
    pub affected: Vec<SemanticId>,
}
```

## Hard failures

-   syntax errors
-   type errors
-   undeclared dependencies
-   undeclared effects
-   contract violations
-   illegal capability use
-   out-of-scope mutation
-   invalid secret literal
-   forbidden fallback patterns where policy marks them hard
-   suppression of mandatory verification

## Diagnostic contract

Each diagnostic contains:

``` text
code
severity
category
target semantic ID
violation
expected
observed
repairable
repair_classes
forbidden_repairs
impact
source span
```

## Tests

-   one fixture per diagnostic code
-   deterministic ordering
-   machine-readable JSON output
-   human-readable CLI output
-   verification cannot be bypassed by agent APIs
