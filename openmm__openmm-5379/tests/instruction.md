# Issue

**Title:** Fix default ion separation cutoff in Modeller

## Description

Change the default `Modeller._addIons()` separation cutoff from `0.05 nm` to the documented `0.5 nm`.

`_addIons()` selects water molecules and replaces their oxygen atoms with ions. It is intended to reject a candidate when it is too close to an ion that has already been placed. The previous `0.05 nm` cutoff was too small to affect normal solvent configurations:

- The effective water radii used by `Modeller.addSolvent()` are approximately `0.175-0.179 nm`.
- The minimum periodic oxygen-oxygen distances in OpenMM's bundled pre-equilibrated water boxes range from approximately `0.223 nm` to `0.264 nm`.
- Consequently, candidate water oxygen sites are already much farther apart than `0.05 nm`, so the cutoff could not reject neighboring candidate sites and was effectively inactive.

This is also inconsistent with the `_addIons()` documentation, which specifies a default of `0.5 nm`. The original design  explicitly proposed an `ionCutoff` of `0.5 nm` when introducing the shared ion-placement routine, but it later appeared with a `0.05 nm` default while retaining `0.5 nm` in its docstring, indicating a decimal-place error.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `83e17ff26c9d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.