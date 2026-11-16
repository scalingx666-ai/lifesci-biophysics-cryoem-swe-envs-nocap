# Issue

**Title:** Limit Nose-Hoover kinetic energy reduction to nonzero buffer entries

## Description

This PR implements the fix discussed in issue #5490:

The Nose-Hoover integrator kinetic energy calculation uses the standard energy buffer size, but for small systems, the maximum number of nonzero entries is much less than this and many entries are zero. This reduces the performance of the NH integrator on systems containing less thermostatted atoms than the number of entries in the standard energy buffer size for the given GPU, because all entries are being scanned and summed even when they are guaranteed to be zero by the size of the system.

This fix sets the reduction to only `min(energyBuffer.getSize(), max(nAtoms, nPairs))` entries. For systems where the number of thermostatted particles or pairs is less than the buffer size, this avoids summing the extra zero entries and gives a decent performance increase. When the number of thermostatted particles is larger than the energy buffer size, there is no effect.

For benchmarks, I modified `benchmark.py` to use a Nose-Hoover integrator, and then ran the following benchmarks with `python benchmark_nh.py --platform CUDA --test gbsa,rf,pme,apoa1rf,apoa1pme,apoa1ljpme,amber20-dhfr,amber20-cellulose,amber20-stmv` on an RTX6000 Ada GPU.

This script and the complete results are attached. For small systems there is a performance increase, about 17% for the DHFR system. For larger systems like STMV there is no performance increase, which is expected.

[benchmark_nh.py](https://github.com/user-attachments/files/34144186/benchmark_nh.py)
[nh_fix.json](https://github.com/user-attachments/files/34144187/nh_fix.json)
[nh_original.json](https://github.com/user-attachments/files/34144188/nh_original.json)

AI usage: diagnosing problem, proposed fixes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c62e3c5a9832`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.