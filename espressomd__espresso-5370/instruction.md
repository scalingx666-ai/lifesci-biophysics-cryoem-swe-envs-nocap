# Issue

**Title:** Raise exception on fractal dimension of degenerate clusters

## Description

Cluster::fractal_dimension fits a line through (log diameter, log particle
count) points accumulated over radial shells. A 2-particle cluster (the
normal minimum of pair-based clustering) yields at most one such point:
with dr=0 the loop appends a single point, and with dr large enough that
no second shell forms, no points are appended at all. Passing n<2 points
to gsl_fit_linear, together with the residual division by
log_diameters.size() (which is zero when the vector is empty), produced a
silent [NaN, NaN] result with no diagnostic.

The fractal dimension is mathematically undefined for fewer than two
radial shells, so fabricating a value is wrong. Prevent the degenerate
input by raising a clear std::runtime_error (surfacing to Python as
RuntimeError via the script-interface dispatch, matching the existing
sanity_checks() and no-GSL conventions) before calling gsl_fit_linear.

Adds testsuite/python/cluster_analysis.py coverage that a 2-particle
cluster raises RuntimeError for both dr=0 and a large dr.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fbeb5f913980`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.