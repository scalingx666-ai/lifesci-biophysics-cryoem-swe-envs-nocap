# Issue

**Title:** StructureFactor: at most 1e8 wave vectors

## Description

The direct method keeps every wave vector in memory, 16 bytes each. Its cap of 1e9 allowed 16 GB before raising. It now refuses more than 1e8 (1.6 GB); a set that large already takes hours of sums over the atoms. Found by the review of the C++ kernels.

Test: a 1000 Å box up to 3 1/Å (2.3e8 vectors, 3.6 GB) now raises, besides the larger boxes already tested. The page lists the error. The docs pages that use `StructureFactor` still run.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b16d8a585947`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.