# Issue

**Title:** Use ifelse for CB selection and drop BenchmarkTools dependency

## Description

## Summary
- switch CB residue selection branches to use `ifelse` for GLY handling instead of ternary syntax
- remove BenchmarkTools from the package dependency and compatibility lists while keeping benchmark usage external

## Testing
- julia -q -e 'using Pkg; Pkg.activate(temp=true); Pkg.develop(path=pwd()); Pkg.add("BenchmarkTools"); include("/tmp/judge.jl")'
- julia --project -e 'using Pkg; Pkg.test()' *(fails: downloadpdb test errors with HTTP 503 from rcsb.org)*

------
[Codex Task](https://chatgpt.com/codex/tasks/task_b_691f49a78c98832d8e4ab5cf58fcb0ba)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `759294f5a83a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.