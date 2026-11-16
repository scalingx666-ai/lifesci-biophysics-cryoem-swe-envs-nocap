# Issue

**Title:** Add Pfam tests for dedicated SIFTS caches

## Description

## Summary
- extend the Pfam SIFTS helper tests to clear the new Pfam/UniProt cache dictionaries before use
- verify the cached mappings include the expected Pfam and UniProt entries while filtered results only return overlapping chains
- confirm repeated lookups reuse cached data without mutating the stored keys

## Testing
- julia --project -e 'using Pkg; Pkg.test(coverage=true)'

------
[Codex Task](https://chatgpt.com/codex/tasks/task_b_6914ba4249f4832db04c5db9191c3506)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `104144646702`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.