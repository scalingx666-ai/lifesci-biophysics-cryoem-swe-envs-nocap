# Issue

**Title:** Fix percentidentity threshold for XAA columns

## Description

## Summary
- update `percentidentity` so the XAA branch keeps the mismatch threshold in sync when skipping ignored columns
- add a regression test covering mixed XAA and mismatches at a passing identity threshold
- clarify the regression test comment to note the 2/3 (66%) counted columns versus the ignored 2/4 (50%) length

## Testing
- `julia --project -e 'using Pkg; Pkg.test(coverage=true)'` *(fails: ftp.ebi.ac.uk connection error during SIFTS download)*
- `julia --project -e 'using Test, MIToS, MIToS.MSA; @testset begin @test percentidentity(res"AXAA", res"AAGA", 60.0) end'`

------
https://chatgpt.com/codex/tasks/task_b_68c9865c8fa4832d8722cc14f697d567

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `537e78504f71`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.