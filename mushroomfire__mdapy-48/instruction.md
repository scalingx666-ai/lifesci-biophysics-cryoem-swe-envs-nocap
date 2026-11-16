# Issue

**Title:** Raise where a pair of atoms reaches the cutoff between the cells of a quasi-harmonic calculation

## Description

`quasi_harmonic` and `thermal_elastic_constants` (with the quasi-harmonic method) now relax every cell first, before any phonons. They then raise `InvalidParameter` if a pair of atoms comes within `displacement` of the cutoff of the calculator in some of those cells and not in others. The message gives the two lengths and where they occur, and suggests a narrower range of volumes, a smaller strain or another potential.

**Why.** The second derivative of NEP's cutoff function, 0.5 (1 + cos πr/r_c), is not zero at r_c, and neither is that of tabulated EAM potentials. So the force constants of a pair jump when it crosses the cutoff. Finite displacements spread that jump over ±`displacement`, which is why the margin is needed.

**Example.** hcp Mg with the 16-metal NEP model, cutoff 6 Å: the shell at √(a² + c²) is 6.078 Å at zero stress, and 6.017 Å in the most compressed cell of the default grid. C33 at 600 K by strain step:

| strain | before | now |
|---|---|---|
| 1e-3 | 54.4 GPa | 54.4 GPa |
| 2.5e-3 | 49.2 GPa | raises: 6.0007 Å apart in one strained cell, 6.2378 Å in another |
| 5e-3 (default) | 32.8 GPa | raises |
| 7.5e-3 | 42.0 GPa | raises |

With the volumes limited to 0.995 to 1.09, no pair comes near the cutoff, and C33 is 54.4 GPa at every strain. The quasi-static method has no force constants and is not checked.

**Tests**
- The fifth shell of the synthetic EAM copper crosses its 5 Å cutoff below 0.89 of the volume. `quasi_harmonic` now raises for volumes from 0.85 to 1.0, and `thermal_elastic_constants` raises at a strain of 0.04; the quasi-static method still runs.
- `_crossing` is tested on scaled fcc cells: with and without the margin, and with an atom wrapped to the other side of the box.
- All 128 documentation pages still run, and so do the 173 lattice-dynamics tests.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2eef2f1113a7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.