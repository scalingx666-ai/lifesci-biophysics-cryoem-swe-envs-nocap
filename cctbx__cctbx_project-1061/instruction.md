# Issue

**Title:** Added output.write_files Phil parameter to probe2 and reduce2 to enab…

## Description

…le harnessing the programs without having them write their output file(s). Modified reduce2 to harness probe2 this way rather than creating and deleting a temporary file.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `db7e7c87b06b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.