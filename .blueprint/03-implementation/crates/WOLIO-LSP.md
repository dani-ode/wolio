# wolio-lsp

## Responsibility

Expose compiler and semantic information through Language Server
Protocol.

## Initial features

-   diagnostics
-   go to definition
-   find references
-   hover
-   symbol outline
-   semantic search
-   impact preview

## Important design

LSP consumes the same semantic APIs as `wolio-agent`.

There must not be a second semantic implementation for editor tooling.

## Tests

-   document lifecycle
-   diagnostics
-   symbol navigation
-   semantic query results
