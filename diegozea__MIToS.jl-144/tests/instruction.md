# Issue

**Title:** Optimize interaction key mapping

## Description

## Summary
- refine `_add_CTER_O!` preallocation and avoid `union!`
- preallocate per-residue sets in `_generate_dict!`
- rely on `_generate_dict!` hints in `_generate_interaction_keys`
- enlarge C-terminal oxygen estimate using `20 * 3`

## Testing
- `julia --project -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("PDB"); MIToSTests.retest("PDB")'`
- `julia --project -e 'import PkgBenchmark, MIToS; PkgBenchmark.benchmarkpkg(MIToS; retune=true)'`

------
https://chatgpt.com/codex/tasks/task_b_686395026c28832da46a9237f33d2fb3

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `244ec2b8bd95`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.