# Issue

**Title:** Add benchmarks for internal functions

## Description

## Summary
- set up VCat benchmarks for vertical concatenation
- cover MMCIF, SIFTSResidue, interaction keys and matched Cαs in the suite
- include new benchmarks in `benchmark/benchmarks.jl`

## Testing
- `julia --project -e 'using JuliaFormatter; format("benchmark", verbose=false)'`
- `julia --project -e 'import PkgBenchmark, MIToS; PkgBenchmark.benchmarkpkg(MIToS; retune=true)'`

------
https://chatgpt.com/codex/tasks/task_b_68629114a990832d9ba418466b94e3ab

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `57c8b6868ef4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.