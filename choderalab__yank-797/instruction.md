# Issue

**Title:** New automatic parameterization scheme for FlatBottom and various small fixes

## Description

- Save NaN logs into experiment folder. This avoids multiple experiments running in parallel to overwrite files generated for previous failures.
- Setup all systems even if experiments are not specified. This way it's easier to use the pipeline just to setup up systems.
- Fix experiment log file name when running a list of experiments in the YAML script.
- Raise an error if restraint shell volume is bigger than system volume when computing the standard state correction.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `907a80e70aee`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.