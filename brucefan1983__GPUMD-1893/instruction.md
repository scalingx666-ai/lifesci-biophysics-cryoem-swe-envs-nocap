# Issue

**Title:** Fix VC-SGC bias increment constant

## Description

## Summary

.

Correct the constant term in the VC-SGC bias energy increment from `+1.0` to `+2.0`.

## Changes

For a transmutation from species a to species b, both changing species contribute +1 to the finite difference of the quadratic bias:

(N_b + 1)^2 - N_b^2 = 2N_b + 1
(N_a - 1)^2 - N_a^2 = -2N_a + 1

Their sum therefore contains +2. This PR changes only that constant.

## Validation

Both the unchanged official source (commit `8bf5301`) and the version containing only this fix were compiled and tested on NVIDIA V100 GPUs.

NEP Zr-Cu liquids with 1000 and 2000 atoms were simulated using identical inputs within each pair, with a thermostat target of 2000 K and kappa = 1000. Each run lasted 200 ps; the first 20 ps were excluded from analysis.

| Atoms | Cu count SD with +2 | Cu count SD with +1 |
|---|---:|---:|
| 1000 | 0.460 | 0.528 |
| 2000 | 0.708 | 0.732 |

SD is the standard deviation of the Cu atom count, in atoms, not a standard error. In these tests, the +1 rule gave a distribution about 15% wider for 1000 atoms and about 3% wider for 2000 atoms.

These percentages describe the observed MD+MC runs under the stated thermostat settings, not a universal error estimate. Input files and raw results are available on request.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8d03890fa033`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.