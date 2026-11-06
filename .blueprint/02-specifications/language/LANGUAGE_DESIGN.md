`.blueprint/02-specifications/language/LANGUAGE_DESIGN.md`
# Wolio Language Design V1

> Revision 1.1 resolves token spellings, precedence, imports, and grammar in
> CORE_V1.md; version negotiation is in DELIVERY_POLICY.md. The OPEN section
> below describes the superseded draft. M01 parses; M02 executes; M03 guards.

**Status:** historical/non-normative; CORE_V1.md is the V1 language authority
**Authority:** `LANGUAGE_SPEC.md` remains the detailed language authority.

V1 uses explicit declarations for modules, types, entities, operations, errors,
contracts, dependencies, effects, and tests. Parsing is deterministic and
produces a stable AST; semantic identity is separate from source location.
The grammar is versioned and additions must define syntax, semantics,
diagnostics, and compatibility impact together.

## M01 subset

M01 implements the smallest module/declaration subset needed for an empty
module, hello-world program, and deterministic syntax errors. Result/Option,
contracts, effects, and dependencies are design commitments for the semantic
milestones and must not be silently approximated in M01.

## OPEN

Concrete token spellings, precedence, import syntax, and version-negotiation
rules require approval against `LANGUAGE_SPEC.md` before implementation.
