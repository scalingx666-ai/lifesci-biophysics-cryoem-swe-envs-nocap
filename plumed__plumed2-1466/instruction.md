# Issue

**Title:** Validate reactive Voronoi pair geometry across MPI ranks

## Description

## Summary

Validate reactive Voronoi pair geometry before distance derivatives are evaluated. Diagnostics distinguish invalid coordinates/cells, neighbor-list mappings, zero separation and overflowing scores, and report the relevant atom pair and rank.

Errors found by workers are collected before reporting, so a bad pair on one MPI rank or OpenMP thread does not leave the other ranks in a collective. Valid-input formulas and neighbor-list behavior are preserved.

## Tests

An independent current-master MPI build passed the existing reactive Voronoi and skin regressions plus new serial and multi-rank invalid-distance regressions. The new cases cover non-finite coordinates, degenerate/non-finite cells, zero separation, score overflow, and collective error handling. Native astyle and whitespace checks passed.

This follows the already merged reactive Voronoi implementation (#1442); it contributes only validation and diagnostics, with no sampling or performance claim.

## Python/OpenMP CI follow-up

Includes the separately reviewable regression-process lifetime fix #1470, which retains the dynamically loaded kernel during Python shutdown. It preserves two-thread execution and the original numerical reference. This addresses the rt-symfunc shutdown failure observed on the debug runner.

Other observed CI failures are distinct: `codecheck` timed out downloading GNU Autoconf; coverage ran 829 tests with zero numerical errors but rejected seven unavailable optional-dependency tests; Intel DRR extraction differed in one final decimal (19.765836219 vs 19.765836220) in an unchanged module. No dependency gate or numerical reference was relaxed.

## Regression follow-up

Included the accepted `rt-protein` shutdown fix #1473, the `rt-grid` follow-up #1477, and the timestep-test initialization fix accepted in #1464. The Python configs pass three direct two-thread runs per case with byte-identical reference output; the existing build reports Python unavailable to the native runner. The timestep regression passes through the native runner with poisoned automatic-variable initialization against both v2.10 and master kernels. No reference values, tolerances or thread counts were changed. The Autoconf download failure has a separate upstream CI proposal in #1474.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f3d17f6dd892`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.