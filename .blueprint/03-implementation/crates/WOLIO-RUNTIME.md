# wolio-runtime

## Responsibility

Provide the minimal runtime ABI required by generated native programs.

## Principles

-   no mandatory GC
-   no VM
-   no reflection by default
-   minimal dynamic dispatch
-   native execution

## Initial runtime services

-   process exit
-   stdout/stderr
-   memory helpers if required
-   Result/Option support if representation needs runtime helpers
-   task/channel primitives only when concurrency is implemented

## ABI rule

Runtime ABI is versioned independently and must be pinned by
`wolio.lock`.

## Tests

-   ABI compatibility
-   stdout
-   error propagation
-   memory safety integration tests
