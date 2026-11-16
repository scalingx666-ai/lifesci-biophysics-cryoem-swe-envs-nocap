# Issue

**Title:** Remove L-W ixn group and merge w/P-L group

## Description

- Remove the L-W specific ixn group and merge it back in with the P-L (now env-L) ixn gorup.
- This should improve performance
- Maintain backward compatibility with FFs that have a separate 'solvent' section if the parameters match those of the original ixn group

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ea7af897d365`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.