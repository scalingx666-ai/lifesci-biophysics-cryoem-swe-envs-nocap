# Issue

**Title:** Update coot.desktop

## Description

- Added `StartupWMClass` in coot.desktop in order not to spawn cloned icons upon launching flatpak  Coot
- Updated `Comment` in coot.desktop, which is used on Flathub

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `76edd966edfe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.