# Issue

**Title:** Precompile `read(filename, PDB)`

## Description

This greatly reduces the latency of the first read.

This times the first read without using `@time`, as that can force compilation prior to starting the timer.

## master

```julia
julia> tstart = time(); read("somefile.pdb", PDB); time()-tstart
3.2666900157928467
```

## this branch

```julia
julia> tstart = time(); read("somefile.pdb", PDB); time()-tstart
0.04239511489868164
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ef76d43cbfa7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.