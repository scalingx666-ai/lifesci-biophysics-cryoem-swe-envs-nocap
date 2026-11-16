# Issue

**Title:** Fix thread race in `AlchemicalPartition` from shared interaction objects

## Description

## Summary

`assemble_mbar_inputs` (the partitioned path) had a data race: every per-thread `PartitionedReducedPotentialWorkspace` shared the same mutable interaction objects, so concurrent energy evaluations raced on them. This fixes it by deep-copying the interactions when the partition builds its systems.

## Problem

`AlchemicalPartition` extracts the master/λ interaction lists from the input states by reference and passes them into `System(deepcopy(ref_sys); general_inters=(master_gils...,), ...)`. The `System` copy constructor does not deep-copy interactions handed to it as keywords, so the deep-copied interactions inside `deepcopy(ref_sys)` are discarded and `master_sys` / each `λ_sys` end up holding the original objects. `assemble_mbar_inputs_partitioned` then creates one workspace per thread, each sharing those objects, so multiple threads mutate them concurrently in `fill_mbar_inputs_partitioned!`. This is most visible with `PME` (which mutates its grids and executes FFT plans), but it affects any interaction with mutable per-evaluation state — e.g. `ANIPotential` (`buffers::Ref{Any}`) and `AbstractGBSA`.

## Fix

Deep-copy the interaction lists when constructing `master_sys` and the `λ_systems` in `AlchemicalPartition`:

```julia
pairwise_inters      = deepcopy((master_pils...,)),
general_inters       = deepcopy((master_gils...,)),
specific_inter_lists = deepcopy((master_sils_1a..., ...)),
```

`PME` already implements `Base.deepcopy`, so no other change is required.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9008e6446d31`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.