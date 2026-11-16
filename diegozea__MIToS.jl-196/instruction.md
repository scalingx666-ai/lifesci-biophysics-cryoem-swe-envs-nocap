# Issue

**Title:** Add exact threaded Hobohm I

## Description

## Summary

This PR adds an exact threaded implementation of Hobohm I in `src/MSA/Hobohm.jl` while keeping the outer representative loop serial and threading only the inner scan over candidate sequences for a fixed representative.

The threaded implementation is available through the existing public API via the `threads::Bool` keyword. Threading remains explicit opt-in (`threads=false` by default), so existing callers do not pay thread startup overhead unless they ask for it and custom predicates are not run concurrently unless the caller opts in.

## Why The Implementation Is Exact

The algorithm still processes representatives in the same serial order as the original implementation. For a fixed representative, the threaded path only parallelizes the candidate scan over `j`; each iteration evaluates the predicate for one candidate and, if it matches, assigns that candidate to the current representative's cluster.

A follow-up refactor moved `clustersize` construction out of the representative loop entirely: the core pass now computes only the final cluster assignment vector, and `clustersize` is derived afterward with a single linear pass over those assignments. That keeps the results identical while simplifying the shared serial/threaded core.

## Why It Is Thread-Safe

The outer representative loop is still serial, so at any moment there is only one active representative and one active `cluster_id`. Inside the threaded candidate scan, each iteration touches a distinct `j`, so direct `cluster[j] = cluster_id` writes do not contend with each other.

There are no shared counter updates inside the threaded region, no `threadid()`-indexed mutable state, and no `:static` scheduling. `clustersize` is built afterward in a separate serial pass.

## API

The public API adds only a `threads::Bool` keyword. It does not expose integer tuning parameters such as `min_parallel`, `ntasks`, or chunk sizes. Threading is enabled only when the caller passes `threads=true` and worker threads are available.

## CI Coverage

The existing Hobohm threaded-equivalence tests are now exercised in GitHub Actions as well. The main CI workflow sets `JULIA_NUM_THREADS: 4,1` for the `julia-actions/julia-runtest@v1` step, so `hobohmI(...; threads=true)` no longer silently falls back to the serial scan during coverage runs.

## Validation

Commands run locally in a multithreaded Julia session:

```bash
julia --project --threads=auto -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("MSA"); MIToSTests.retest("Hobohm I"); MIToSTests.retest("Hobohm I")'
julia --project --threads=auto -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("Gaps"); MIToSTests.retest("Gaps")'
julia --project --threads=auto -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("Information"); MIToSTests.retest("CorrectedMutualInformation"); MIToSTests.retest("CorrectedMutualInformation")'
```

Results:

- `MIToSTests: Hobohm I | 28` on both targeted runs.
- `MIToSTests | 249` on both `Gaps` runs.
- `MIToSTests: Information | 3`, then `MIToSTests: CorrectedMutualInformation | 55` on both targeted runs.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2cc27802672c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.