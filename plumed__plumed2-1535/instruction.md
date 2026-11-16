# Issue

**Title:** UPPER_WALLS/LOWER_WALLS: no PBC on the difference with AT

## Description

For periodic arguments the difference between the argument and AT was wrapped into the periodic domain, so each wall acted on half of the domain: e.g. walls at +-2 on a torsion pushed it out of [-2,2] when it was beyond +-(pi-2). The wall now acts on the value of the argument in its domain, both for scalar arguments and for vectors (the shortcut uses CUSTOM instead of COMBINE, which applies PBCs).

For periodic arguments AT must be within the domain, and a warning about the discontinuity at the domain boundary is written in the log. Documentation, CHANGES and a new regtest (rt-WALLS-periodic) are added.

Claude-Session: https://claude.ai/code/session_016vwxtUcU4SHYbwm9M3GRCB

this solve #658

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fd26c15e7fee`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.