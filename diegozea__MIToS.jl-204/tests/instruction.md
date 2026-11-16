# Issue

**Title:** Fix MIToS precompilation on Julia 1.13

## Description

MIToS 3.10.0 and current master resolve their dependencies on Julia 1.13, but fail to precompile with `Inconsistent dictionary sizes` while initializing `BLOSUM62_Pij`.

The contingency-table constructor relies on `@ntuple N k -> "Dim_k"` expanding the string to distinct labels. Julia 1.13 leaves the string literal unchanged: for two dimensions it produces `("Dim_k", "Dim_k")`, and the marginal-label dictionary collapses to one key for a two-column array. This behavior was reproduced on Julia 1.13.0 and compared with Julia 1.12.7.

Build both table dimension names and marginal column names explicitly with `string("Dim_", k)`. The production fix changes two expressions. Extend the existing constructor tests to check the exact labels for one, two, and three dimensions across ungapped, gapped, and reduced alphabets.

Authoritative references: Julia's [1.12.7 Cartesian implementation](https://github.com/JuliaLang/julia/blob/v1.12.7/base/cartesian.jl) dispatches `lreplace!` on string literals; the [1.13.0 implementation](https://github.com/JuliaLang/julia/blob/v1.13.0/base/cartesian.jl) preserves them and performs suffix rewriting through symbol handling. The [Cartesian documentation](https://docs.julialang.org/en/v1/devdocs/cartesian/#Anonymous-function-expressions-as-macro-arguments) documents substitution of the anonymous-function argument, which the explicit construction uses.

Validation:

- Reproduced the original precompilation failure with the latest Git tag, master, and registry release in fresh environments.
- The exact pushed commit (`4ba2a621`) installs from GitHub in a fresh environment, passes strict precompilation, loads, and passes three smoke assertions on Julia 1.13.0.
- Full `Pkg.test("MIToS"; coverage=true)` on Julia 1.13.0 with CI's `JULIA_NUM_THREADS=4,1`: passed, including doctests.
- Contingency-table tests on Julia 1.12.7 and 1.13.0: 532 passed per run, run twice on each version.
- JuliaFormatter 2.14.0 and `git diff --check`: passed locally.
- GitHub CI: all six full-test jobs pass (Julia 1.9 and 1.13.1 on Linux, Windows, and macOS). Documentation, formatting, Codecov, and Coveralls checks also pass.

To satisfy CI's current formatter, a separate commit applies quote escaping and removes redundant semicolons in 12 existing files; parsing every before/after file with Julia and removing line-number nodes confirmed identical syntax trees. This keeps the functional fix separately reviewable.

The benchmark comparison fails while loading **unpatched master** on Julia 1.13, before comparing the PR. This is the original bug fixed here; the comparison cannot run on that baseline until the fix reaches master. Its log reports the original contingency-table exception during baseline benchmark-script discovery. [Baseline failure log](https://github.com/diegozea/MIToS.jl/actions/runs/36637168926/job/109640531218).

Because the benchmark workflow uses `pull_request_target`, GitHub reads it from the default branch; a workflow adjustment confined to this PR would not affect this check before merge. See the [GitHub event documentation](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#pull_request_target).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0427f079adcc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.