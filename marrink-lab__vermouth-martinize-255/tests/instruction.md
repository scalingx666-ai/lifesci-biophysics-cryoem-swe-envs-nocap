# Issue

**Title:** fix dihedral treatment

## Description

Yet another parser issue. Link dihedrals are added to the last block in addition to themselves instead of as dihedrals and impropers to the link interaction. I fixed it. As far as I can tell it works now. But we really should compare some protein itps to previous versions. This issue only exists since the last fix and reshuffling of the dihedral treatment that is PR #251 .

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `92f49ddda1fe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.