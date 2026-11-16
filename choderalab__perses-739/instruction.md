# Issue

**Title:** fix CI.yaml

## Description

#682 introduced parsing errors in `.github/workflows/CI.yaml`: `matrix.cfg.os` in this [line](https://github.com/choderalab/perses/blob/master/.github/workflows/CI.yaml#L17) is no longer defined in the [matrix](https://github.com/choderalab/perses/blob/master/.github/workflows/CI.yaml#L20).

Thus, tests are failing for subsequent PRs.

I fixed the `matrix.cfg.os` lines based on , however, the current CI.yaml in master has other differences from that commit, so I'm not sure if there are more undesired changes introduced by #682.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `086bd714de51`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.