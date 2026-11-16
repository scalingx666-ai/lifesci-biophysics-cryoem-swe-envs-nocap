# Issue

**Title:** Remove netCDF4 dependency, set up PyPI build variants

## Description

Removing netCDF4 as a hard dependency (see ).

While I was at it, I added in `mdtraj[tests]` and `mdtraj[optional]`, which should allow you to install most of the other dependencies without having to resort to the test conda env recipe, and `pip install mdtraj[all]` should install both sets. I also updated the `delay_import` documentation to remove `scripttest` (not used anymore) and `scipy` (a hard dependency now).

I believe we are dropping python 3.9 (EOL later this year) so did that in the metadata as well.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `500d31ed2804`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.