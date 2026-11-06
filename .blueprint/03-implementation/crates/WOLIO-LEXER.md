`.blueprint/03-implementation/crates/WOLIO-LEXER.md`
# wolio-lexer

## Responsibility

Convert source text into loss-aware tokens with deterministic source
spans.

## Inputs

-   UTF-8 source text

## Outputs

-   `TokenStream`
-   `LexError`

## Core types

``` rust
pub struct Span {
    pub start: u32,
    pub end: u32,
}

pub struct Token {
    pub kind: TokenKind,
    pub span: Span,
    pub lexeme: Symbol,
}

pub struct TokenStream {
    pub tokens: Vec<Token>,
}
```

`TokenKind` covers V1 keywords, identifiers, literals, punctuation,
operators, comments, and EOF.

## Required behavior

-   deterministic tokenization
-   no parser knowledge
-   preserve exact spans
-   reject invalid UTF-8 only at input boundary
-   reject malformed numeric/string literals
-   no semantic interpretation

## Errors

``` text
LEX001 invalid character
LEX002 unterminated string
LEX003 invalid numeric literal
LEX004 invalid escape
```

## Tests

-   every token family
-   span correctness
-   malformed literals
-   Unicode identifiers if V1 allows them
-   deterministic repeated lexing

## Non-goals

-   type checking
-   symbol resolution
-   formatting
-   AST construction

## Done when

All parser fixtures can be lexed deterministically and diagnostics
always point to stable source spans.
