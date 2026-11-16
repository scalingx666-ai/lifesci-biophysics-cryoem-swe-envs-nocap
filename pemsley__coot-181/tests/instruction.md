# Issue

**Title:** Fix desktop files

## Description

- Revert `Comment` in coot.desktop
- Revert `Icon` in coot.desktop
- Rename pixmaps/icons/io.github.pemsley.coot.svg to pixmaps/icons/coot.svg

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f708dfc9cf5f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.