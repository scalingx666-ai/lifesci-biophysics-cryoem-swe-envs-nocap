# Issue

**Title:** Fix zero-count receives from empty domain-decomposition ranks

## Description

When domain-decomposition rank 0 fills the receive buffer, a later empty rank still sends its zero-length messages. `Atoms::wait()` forms the receive pointer with `vector::operator[]` at `size()`, which is undefined behavior and is rejected by a debug standard library.

This uses `data() + offset` only when capacity remains, otherwise passing a null pointer with count zero. The messages, tags, order and force distribution are unchanged. The native two-rank regression covers both empty-rank layouts and a balanced layout, synchronous/asynchronous exchange, and first/subsequent steps against analytic energy and forces. This does not depend on OPES or an MD-engine interface.

Validation on the current v2.9 base: release and `--enable-debug --enable-debug-glibcxx` MPI builds pass `make nmcheck`, all 12 analytic cases, and `rt-mpi2`, `rt-mpi2sync`, `rt65-mpi` and `rt39-mpi`, with `PLUMED_ALL_TESTS=yes`. The same native reproducer previously failed on the unpatched debug build. The full optional-dependency suite was not run.

Targeting v2.9 as the oldest affected maintained branch, following the contribution guidelines.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `34758256bd99`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.