# Issue

**Title:** Streamline Stockholm annotation parsing tests, closes #168

## Description

## Summary
- call the annotation fragment helper directly when parsing GC metadata
- use `mktemp` when writing temporary Stockholm files in IO tests
- deduplicate HMMER multi-block annotation checks by iterating over the alignments

## Testing
- julia --project -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("IO"); MIToSTests.retest("IO")'

------
https://chatgpt.com/codex/tasks/task_b_69028cfaef64832da76e03e9e13f987e

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a66d28950f8e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.