# Issue

**Title:** Implement newton solver for MBAR

## Description

`iterate_mbar` solves the MBAR equations by self-consistent iteration (Shirts & Chodera 2008, Eq. C3). On long ladders, where neighbouring states overlap poorly, this takes thousands of iterations. Its stopping rule, a relative change in `f`, can also stop before the weights are normalised when `|f|` is large.

This PR adds `method=:newton`, the Newton–Raphson solver of the same paper's Appendix C2.

- Levenberg–Marquardt damping, accepting a step only if it lowers the convex MBAR objective;
- a fall-back to a self-consistent step when a Newton step fails;
- a stopping test on the normalisation of every sampled state's weights (`atol`).

It warns if it stops at `max_iter`, or at the floating-point limit before reaching `atol`.

### Changes

- `iterate_mbar(u, win_of, N_counts; method=:fixed_point, f_init=nothing, rtol=1e-8, atol=1e-10, max_iter=10_000)`
  - `method`: `:fixed_point` (the default, unchanged behaviour) or `:newton`.
  - `f_init`: optional initial free energies in any gauge, for example from a previous optimisation step, or from the full data set when solving jackknife blocks.
  - States with no samples (`N_counts[i] == 0`) are now allowed. Their free energies follow once the sampled states are solved.
  - Argument checks moved into a shared helper, `mbar_check_counts`.
- `mbar_weights` and `pmf_with_uncertainty` throw an `ArgumentError` if any state has no samples, rather than failing later.
- The `MBARResult` constructors forward their keyword arguments to `iterate_mbar`, so `method`, `f_init` and `atol` can be used there too.
- Docs: a short paragraph and example in `docs/src/free_energy.md`.

### Benchmark

A temperature ladder with a Gaussian density of states, sized like 887 water molecules at 298 K. Single thread, best of 3. The reference f is the fixed point run to `rtol=1e-14`.

| States × frames | Solver | Iterations | Time | Normalisation residual | Max error in f (kT) |
|---|---|---|---|---|---|
| 25 × 4,000 | fixed point | 715 | 0.78 s | 3e-5 | 3e-3 |
| | Newton, cold | 14 | 0.023 s | 3e-12 | 4e-9 |
| 48 × 4,800 | fixed point | 2,450 | 6.2 s | 6e-5 | 3e-2 |
| | Newton, cold | 17 | 0.057 s | 5e-12 | 3e-8 |
| 48 × 24,000 | fixed point | 2,454 | 31 s | 6e-5 | 3e-2 |
| | Newton, cold | 19 | 0.32 s | 3e-12 | 3e-8 |

Warm starts with `f_init`, on the same three data sets:

| States × frames | Re-solve at perturbed energies (u × 1.002) | Jackknife block (80% of the frames) |
|---|---|---|
| 25 × 4,000 | 4 iterations, 5 ms (fixed point 0.77 s) | 3 iterations, 3 ms (fixed point 0.62 s) |
| 48 × 4,800 | 4 iterations, 11 ms (fixed point 6.2 s) | 3 iterations, 7 ms (fixed point 5.2 s) |
| 48 × 24,000 | 4 iterations, 52 ms (fixed point 31 s) | 3 iterations, 33 ms (fixed point 26 s) |

- The fixed point's iteration count grows with the number of states. Newton's barely changes.
- With `|f|` around 1e4, the default `rtol=1e-8` stops the fixed point while its free energies are still up to 0.03 kT off, with weights normalised only to 6e-5.

### Tests

A new `MBAR solvers` testset in `test/analysis.jl` covers:

- agreement with the fixed-point solver;
- warm starts in any gauge;
- states with no samples;
- the warnings;
- input validation;
- keyword forwarding through `MBARResult`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f15492143b2d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.