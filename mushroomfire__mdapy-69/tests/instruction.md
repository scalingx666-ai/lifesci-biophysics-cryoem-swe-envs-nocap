# Issue

**Title:** thermal_conductivity: the scattering rates in C++

## Description

Part of step (e): the speed of the thermal conductivity. It builds on #68 (mode Grüneisen) and #67; this branch holds their commits until they merge.

## What changes

`mdapy._core.analysis.three_phonon_rates` is one kernel that does the whole inner loop of `thermal_conductivity`:
- the contraction of the third-order force constants of q′ and q″ with the modes of q, q′ and q″, in three O((3n)⁴) steps;
- |Φ|²/(ωω′ω″);
- the deltas, either Gaussians computed inline or weights given by the caller (prepared for the tetrahedron method of the next PR), weighted by the occupations of every temperature.

Further details:
- It runs in parallel over q′ with `parallel_reduce`, so the sums are folded in chunk order and do not depend on the number of threads.
- Complex arrays cross as float64 pairs.
- The NumPy side keeps the phases and the mesh bookkeeping.

## How it is shown to be right

- The phono3py comparisons are unchanged: κ and every γ for fcc, hcp and L1₂ at three temperatures, to 1e-7. So are all the other conductivity tests.
- New: the results with 1 thread and with 4 threads are identical bit for bit.
- The kernel compiles cleanly under GCC 15 at -O3 with `-Wall -Wextra -Wpedantic -Wshadow -Werror`, and for ThreadSanitizer. The full suite passes.

## Speed

L1₂ (4 atoms) on an 8³ mesh drops from 9.9 s to 0.96 s, in the range of phono3py's C code; fcc on 9³ takes 0.1 s. The page now says how the time grows: with the irreducible points times the mesh, and as the fourth power of the modes of the cell.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bcc2eb2d305d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.