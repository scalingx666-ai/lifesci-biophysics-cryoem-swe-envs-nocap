# Issue

**Title:** Migrate to JSON.jl 1.0 APIs

## Description

## Summary
- replace the JSON3 dependency with JSON in the project configuration
- update MIToS.PDB to import JSON and parse AlphaFoldDB/PDB GraphQL responses with JSON.parse
- refresh AlphaFold documentation to reference JSON.Object instead of JSON3.Object

## Testing
- julia --project -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("PDB"); MIToSTests.retest("AlphaFoldDB Tests")'

------
https://chatgpt.com/codex/tasks/task_b_690b461a45fc832d9f2ce1d67bfba0aa

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `194465286cbc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.