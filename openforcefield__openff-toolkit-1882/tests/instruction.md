# Issue

**Title:** Streamline testing of beta/RC upstreams

## Description

- [x] Add MDTraj's RC label
- [x] Use the same environment and vanilla CI
  * https://github.com/mamba-org/setup-micromamba?tab=readme-ov-file#custom-condarc-file
- [x] Do not run or upload test coverage
- [x] Do not run at midnight UTC
- [x] Run tests against this branch
  * https://github.com/openforcefield/openff-toolkit/actions/runs/9291590333

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ee4576832d77`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.