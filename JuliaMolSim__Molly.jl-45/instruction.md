# Issue

**Title:** up interface for celllistmap 0.7.0

## Description

Dear @jgreener64, I have updated the interface of the CellListMap neighbor finder to the 0.7.0 version, which has some breaking changes (hopefully the last ones until 1.0).

The changes are:

- Now CellListMap propagates units properly through the code, so no need anymore of stripping units before passing the data to it. Because of that, one less type parameter is necessary in the constructor as well. ([see here](https://m3g.github.io/CellListMap.jl/stable/units_etc/)). I have added an `number_of_batches` option to the constructor, although I don't think it will be  used in practice. Since the number of batches is not anymore necessarily equal to the number of threads, the preallocated output arrays do not have length `nthreads()`, and thus their resetting is performed directly on their length (actually this is better and can cooperate with nested multithreading much more).

- The number of batches used in parallel calculations are defined by an heuristic (or can be set by the user), to improve the performance particularly if many threads are being used. The new approach avoids some latency problems, uses less memory and avoids some huge GC workloads particularly when parallelizing over many threads. ([see here](https://m3g.github.io/CellListMap.jl/stable/parallelization/))

Tests are passing here. 

Performance should not be significantly affected, but for specific, particularly small (~10^4 particles) you may experience small slowdowns, maybe, but nothing really important. Anyway, the bottlenecks are now much more clear and any improvement on this side will be performed from v0.7 on.

The code, particularly because the units can be propagated through CellListMap, is simpler now.

Happy new year.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `48a97137917d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.