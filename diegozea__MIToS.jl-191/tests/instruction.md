# Issue

**Title:** Fix MMCIF roundtrip writing, closes #190

## Description

## Summary
- align MMCIF printing defaults with reader expectations so write_file produces label-based mmCIF output
- add a read/write/read roundtrip test for MMCIFFile to guard against regressions

## Testing
- julia --project -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("PDB"); MIToSTests.retest("PDB")'

------
[Codex Task](https://chatgpt.com/codex/tasks/task_b_693add1d5d84832d880006573ebcebe4)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4a26a3d208d9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.