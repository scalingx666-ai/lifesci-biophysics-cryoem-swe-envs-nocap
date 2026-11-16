# Issue

**Title:** Performance improvements

## Description

Hi!
I noticed that there some performance improvements that can be made. For example due to `atoms::Vector{<:Any}` a lot of dynamic dispatch was occurring inside force calculations. Adding another type parameter solved the problem and provided a ~40x speedup for Lenard-Jones interactions. I think further improvements can be made.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b7d87b241e84`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.