# Issue

**Title:** Example: a collision cascade in tungsten

## Description

The example promised instead of 1.x's `set_pka`, a function too small to belong in a module.

A primary knock-on atom of 2 keV along [1 3 5] in 54 000 atoms of tungsten. The potential is the 16-metal NEP model of the high-throughput example, which has a ZBL repulsion from 1 to 2 Å; it runs in LAMMPS as `pair_style nep/kk` on a GPU. The page:
- equilibrates the crystal at 300 K;
- gives the PKA its energy with a dozen lines of NumPy (`knock_on`), which users can copy;
- runs NVE with `fix dt/reset`, at most 0.05 Å per step and at most 1 fs, with the atoms within 1 nm of the faces of the box kept at 300 K by `fix langevin`;
- counts the Frenkel pairs over time with `WignerSeitz` and draws the defects left.

The blocks are skip-test blocks (they need nep/kk and a GPU). They were run as written on a V100 on Triton with `tools/doc_images.py --skipped`.

**Result**: about 20 ps; the time steps grew from 0.2 fs to 1 fs.

| Quantity | Value |
|---|---|
| PKA speed | 458 Å/ps |
| Temperature at the start | 580 K (the 2 keV count as kinetic energy) |
| Temperature at the end | 300 K |
| Frenkel pairs at 0.55 ps | 26 |
| Frenkel pairs after 3 ps | 5 (4 in an earlier run; chaotic) |

The run needs #54 (the time when the time step changes) and #57 (`combine` with `atomic_virials`), both bug fixes that merge before this. The 1.x migration page now points to the example for `set_pka`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ad4032812794`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.