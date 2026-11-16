# Issue

**Title:** Fix TabulatedPotential expression for constant potentials

## Description

A single-point tabulated potential (minval == maxval, force.size() == 1) is
an intentionally supported, constant potential: the constructor only throws
when the force table does not hold exactly one element in this case
(mirrored by testsuite/python/tabulated.py). However the constructor then
computed invstepsize = (size - 1) / (maxval - minval) = 0/0 = NaN, which
poisoned every force()/energy() evaluation and triggered an out-of-bounds
read of the size-1 table in Utils::linear_interpolation
(static_cast<int>(NaN) is undefined, table[uind + 1] reads element [1]).

Fix:
- TabulatedPotential.cpp: guard the invstepsize computation so the
  degenerate denominator yields invstepsize = 0 (mirroring the existing
  minval == -1. sentinel branch). With invstepsize = 0 the interpolation
  collapses to the single constant table value, which is physically correct
  for a one-point table.
- linear_interpolation.hpp: harden the residual latent out-of-bounds read so
  the upper data point is never accessed past the end of a single-point table
  (the term is multiplied by dx == 0 anyway).

Adds src/core/unit_tests/TabulatedPotential_test.cpp covering the degenerate
single-point table and a regular multi-point table.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `df5f869f2035`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.