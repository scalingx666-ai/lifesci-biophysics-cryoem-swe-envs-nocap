# Issue

**Title:** Fix nightly test

## Description

Fixes a nightly test failure introduced by #1309.

* Add `hrex_params` to `params` in call to `run_sims_hrex`. As of #1309, `run_sims_hrex` asserts that `hrex_params` is not `None` because it uses the `max_delta_states` parameter.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `be16478d26a2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.