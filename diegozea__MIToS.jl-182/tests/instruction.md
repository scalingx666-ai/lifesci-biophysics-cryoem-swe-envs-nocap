# Issue

**Title:** Skip Leading Whitespace in get_n_words

## Description

## Summary
- Adjust `get_n_words` to skip leading spaces/tabs so the first word is never empty while preserving existing edge cases and Stockholm parsing behavior.
- Add regression tests covering whitespace-prefixed lines (spaces and tabs) alongside existing cases.

## Testing
- `julia --project -e 'using Pkg; Pkg.test()'`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0a0fa1afdaf5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.