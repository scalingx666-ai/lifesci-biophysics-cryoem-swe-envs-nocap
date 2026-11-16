# Issue

**Title:** hydra search path bug

## Description

Replace `${oc.env:PWD}` with relative paths in the Hydra config search path. This fixes configuration resolution with newer Hydra/OmegaConf versions.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `847f666e785d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.