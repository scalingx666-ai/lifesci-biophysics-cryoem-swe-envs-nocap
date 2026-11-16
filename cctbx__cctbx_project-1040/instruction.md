# Issue

**Title:** Fix MPI on XFEL CI for psana

## Description

Observed some flaky behavior ("Missing hostname or invalid host/port description in business card") with mpirun in Rocky Linux on Azure. Apparently fixed by installing ucx.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a26b89f38c82`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.