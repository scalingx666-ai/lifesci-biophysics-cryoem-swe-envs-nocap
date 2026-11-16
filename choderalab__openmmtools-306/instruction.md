# Issue

**Title:** Update context box vectors before positions in SamplerState.apply_to_context()

## Description

.

Without this fix, explicit solvent simulations are likely to sample incorrect distributions or experience `nan`s when a barostat is in use.

Tagging some folks that may have noticed problems with this issue:
* @andrrizzi @Lnaden in YANK
* @maxentile in MCMC-based sample cache generation (if using `SamplerState.apply_to_context`)
* @pgrinaway in perses

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1c229362520e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.