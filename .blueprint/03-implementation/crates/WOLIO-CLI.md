# wolio-cli

## Responsibility

Provide the single `wolio` executable.

## V1 commands

``` text
wolio init
wolio check
wolio test
wolio verify
wolio build
wolio run
wolio inspect
wolio impact
wolio graph
wolio explain
```

## AI commands

``` text
wolio agent inspect
wolio agent plan
wolio agent apply
wolio agent verify
wolio agent impact
```

## CLI rule

The CLI is an orchestration layer.

It must not duplicate semantic logic from compiler crates.

## Output modes

Every command that can be consumed by an agent must support:

``` text
--format human
--format json
```

Machine-readable output must be schema-versioned.

## Tests

-   command routing
-   exit codes
-   JSON schema
-   deterministic diagnostics
-   end-to-end vertical slices
