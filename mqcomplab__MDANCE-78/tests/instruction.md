# Issue

**Title:** chore(SHINE): make sampling configurable and skip when frac == 1

## Description

- run_shine.py: pass sampling variable into Shine
- shine.py: only apply diversity/quota sampling if frac < 1, otherwise keep full trajectories

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8c5846e6ee8e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.