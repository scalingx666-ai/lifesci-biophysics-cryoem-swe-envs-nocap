# Issue

**Title:** Add multiple AnnotatedSequence constructor tests

## Description

## Summary
- split previous `Additional constructors` block into dedicated testsets
- cover AnnotatedSequence round-trip, matrix conversions, and branch coverage

## Testing
- `julia --project -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("MSA")'` *(fails: Package BioAlignments not found)*

------
https://chatgpt.com/codex/tasks/task_b_685d36285230832da48da96ab7fd9d9d

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `569bf964b987`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.