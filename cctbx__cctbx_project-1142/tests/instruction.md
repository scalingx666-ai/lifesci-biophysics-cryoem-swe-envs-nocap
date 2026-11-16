# Issue

**Title:** XFEL GUI updates fall-winter 2025-26

## Description

- More structured input on Trials tab
- GUI config is selected through env variable CCTBX_XFEL_SETTINGS
- Removed LCLS config panel from rungroup dialog
- Configurable path of lock file for MTZ uploader
- Add timeout when opening database connections

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `836498ee7f3f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.