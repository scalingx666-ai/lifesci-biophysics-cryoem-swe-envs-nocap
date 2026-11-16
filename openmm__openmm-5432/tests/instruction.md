# Issue

**Title:** Scale large molecules in parallel in the Monte Carlo barostat

## Description

The `scalePositions` kernel used by the Monte Carlo barostats on the CUDA, OpenCL and HIP platforms processes one molecule per thread, looping over that molecule's atoms twice. For water and small molecules this is fine, but a protein is a single molecule of thousands of atoms handled by one thread with uncoalesced loads, so the kernel's runtime is set by the largest molecule in the system rather than by the number of atoms. Profiling a 46646 atom protein-ligand system under `nsys` showed `scalePositions` taking 1.06 ms per call for a 6750 atom protein, while a full force evaluation on the same system takes about 0.27 ms. At the default barostat frequency of 25 that adds about 10% to the average cost of an MD step, and it grows linearly with protein size.

This change keeps the one-thread-per-molecule kernel for molecules of up to 32 atoms and adds `scaleLargeMolecules`, which uses a whole work group per molecule: the atoms are summed in parallel with a shared memory reduction to find the center, and the shift is then applied in a parallel pass. Molecules are partitioned by size when the molecule arrays are built, so each kernel runs over a contiguous range and neither launches if it has no work. The threshold of 32 is roughly where a single thread's serial loop over the atoms becomes slower than a work group reduction; it is not tuned finely, since molecules near it cost a few microseconds either way. On the system above the two kernels together take 80 µs per move instead of 1.06 ms, and the MD step goes from 0.419 ms to 0.376 ms.

`computeMolecularKineticEnergy` in the same file has the same one-thread-per-molecule structure, but it is only called from `computePressure()` rather than during stepping, so it is left as is.

`testLargeMoleculeScaling` is added to `TestMonteCarloBarostat.h`: molecules of 1 to 300 atoms, straddling the threshold, are scaled rigidly, and each molecule's center is checked to scale with the box while its internal geometry is unchanged. It passes on Reference, CUDA and OpenCL and fails if the large molecule kernel is not launched. The kernel only uses constructs already present in `computeMolecularKineticEnergy` in the same file, so it should be fine on HIP, but I have not been able to test that platform.

The implementation and profiling for this PR were done with the aid of Claude Code (Opus 5).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5a7a268616b5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.