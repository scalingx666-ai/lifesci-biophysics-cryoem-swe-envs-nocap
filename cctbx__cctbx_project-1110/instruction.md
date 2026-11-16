# Issue

**Title:** Rename XFEL dispatchers

## Description

- Rename cctbx.xfel.stripe_experiment to cctbx.xfel.time_varying_refinement
- Rename cxi.mpi_submit to cctbx.xfel.submit_job

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4c9526a195ab`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.