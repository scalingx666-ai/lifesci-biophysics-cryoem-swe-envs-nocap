# Issue

**Title:** fix: add support for force models in spkpredict

## Description

`spkpredict` was not working well on force models.
fixes:
- allows gradient computation for force predictions (use with enable_grad=True)
- writes idx_m values, so force predictions can be assigned to structure ids (use with write_idx_m=True)
- concatenate results

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `49b2bb554f94`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.