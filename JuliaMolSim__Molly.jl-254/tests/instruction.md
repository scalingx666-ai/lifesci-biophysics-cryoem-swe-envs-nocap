# Issue

**Title:** feat: Add new cutoff type PolynomialCutoff

## Description

## Summary

- Adds `PolynomialCutoff`, a new cutoff strategy that multiplies the potential by the quintic switching function used by OpenMM: `S(t) = 1 - 6t⁵ + 15t⁴ - 10t³`, where `t = (r - r_switch) / (r_cutoff - r_switch)`
- The force includes both `S(r)·F(r)` and a correction term `-S′(r)·V(r)` to stay consistent with the modified potential
- `S` and its first two derivatives are zero at `r_cutoff`, giving C² continuity at both endpoints — unlike `CubicSplineCutoff` (Hermite interpolation from the activation point), this scales the true interaction throughout the switching region without needing to evaluate `V` and `F` at the activation distance

## Test plan

- [X] New entry added to the `Cutoffs` testset in `test/interactions.jl` — checks force and PE at `r = 0.7 nm` (in the switching region) against exact reference values, and verifies zero force/PE at `r = 0.95 nm` and `r = 1.0 nm` (beyond cutoff)
- [X] All 32 cutoff tests pass locally

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `264e28859d3e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.