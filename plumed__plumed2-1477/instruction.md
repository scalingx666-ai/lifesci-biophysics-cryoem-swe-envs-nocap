# Issue

**Title:** Keep the kernel loaded during the rt-grid Python shutdown

## Description

and #1473 for `regtest/python/rt-grid`.

With two OpenMP threads, I reproduced a shutdown SIGSEGV in two of three runs after the output had already matched the reference. The backtrace showed an OpenMP worker in `gomp_barrier_wait_end` while the main thread was unloading the kernel.

Set `PLUMED_LOAD_DLCLOSE=no` in this regression only. Loading the updated config and running the existing Python test directly passed three times with byte-identical `logfile` output. The same check passed for `rt-protein` and `rt-symfunc`. Thread counts, reference files and tolerances are unchanged.

The existing local build reports Python unavailable to the native regression runner; these checks used its separately built Python wrapper. The full CI matrix remains to be checked.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a7654be01cfd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.