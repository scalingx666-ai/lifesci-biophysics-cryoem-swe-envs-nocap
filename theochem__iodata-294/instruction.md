# Issue

**Title:** Fix UnicodeDecodeError when pip installing on Windows

## Description

Change the encoding to fix the error below on Windows.
<img width="743" alt="235242135-bd31f4df-4873-4fdb-9032-9b41bf71cc37" src="https://user-images.githubusercontent.com/4535622/235244194-7071dde8-0feb-4303-aa61-6c3aeff3a555.png">

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `73eee89111bc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.