# Technical Definition of Done

> Revision 1.1: release profiles and exact acceptance are in DELIVERY_PLAN.md.
> Trial release requires M03, full toolchain M07. A release may report negative
> benchmark findings; an improvement claim additionally requires the specified
> statistical/economic threshold. Architecture/security extensions in this
> historical checklist are not mandatory V1 features.

Wolio V1 is not complete when the compiler compiles itself.

It is complete when a realistic multi-module project can:

1.  initialize
2.  parse
3.  resolve
4.  type-check
5.  declare and verify contracts
6.  declare and verify effects
7.  enforce explicit dependencies
8.  enforce architecture policy
9.  expose semantic queries
10. accept structured AI changes
11. enforce change scope
12. verify every mutation
13. report machine-readable diagnostics
14. build native code
15. run native code
16. reproduce the semantic graph after deleting `.wolio/`
17. audit AI maintainability
18. survive multiple AI-generated change cycles

## Final gate

The benchmark must demonstrate that Wolio improves at least one
meaningful correctness metric without making the AI workflow
economically impractical.
