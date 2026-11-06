`.blueprint/06-project-management/BENCHMARK_DESIGN.md`
# Benchmark Design V1

> Revision 1.1: DELIVERY_PLAN.md's benchmark protocol replaces this draft corpus
> and OPEN list. Exact model/hardware are preregistered execution inputs. The pilot
> has 6 tasks × 3 runs; expanded evaluation has 24 tasks × 10 runs per arm.
> Only supported equivalent language features may enter a comparison.

**Status:** historical/non-normative; DELIVERY_PLAN.md is the V1 benchmark authority

Compare the same model and comparable prompts on GPT + Go, Rust, TypeScript,
and Wolio. The initial corpus contains 20–50 tasks covering CRUD,
authentication, payment, idempotency, concurrency, transactions, APIs,
authorization, secrets, refactors, fixes, additions, and regressions.

Record task success, semantic bugs by `AI_BUG_TAXONOMY.md`, regressions, repair
iterations, compiler loops, tokens, wall time, security defects, and concurrency
defects. Publish prompts, fixtures, toolchain versions, and raw results so the
comparison is reproducible. No improvement claim is valid without baseline data.

`OPEN`: model/version, hardware, task weighting, sample size, and statistical
threshold for claiming improvement.
