# Issue

**Title:** PERI: fix peri/eps yield norm and plastic return mapping

## Description

**Summary**

Two latent bugs in the `peri/eps` (state-based elastic-plastic) peridynamic
model are corrected.

1. **Yield norm.**  `compute_DeviatoricForceStateNorm()` built the trial
   deviatoric force state with an extra dilatation factor `theta`
   (`omega*theta/wvolume`), but the force loop's deviatoric force state
   (`omega/wvolume`, line ~306) has no such factor.  The yield surface was
   therefore tested against a different, `theta`-inflated force state, so the
   material yielded much later than intended.

2. **Plastic extension update.**  The per-bond plastic deviatoric extension was
   updated as `edpNp1 + rkNew*deltalambda`, which reduces (uniform case) to a
   gain of `~2/r0` on `(e_dev - edp)` and overshoots/diverges under sustained
   plastic flow.  It now uses the dimensionally consistent radial-return update
   `edpNp1 + (deviatoric_extension - edpNp1)*(1 - sqrt(2*psi)/||t_d||)`, scaling
   the elastic part by the same radial factor applied to the force state.

**Related Issue(s)**

Found while re-implementing the PERI models on the BPM framework (see the
companion `bond_style bpm/peri` pull request #5045), where the corrected forms
are used.

**Author(s)**

Found and fixed by Claude Opus 4.8 (Anthropic) under the direction of Joel
Clemmer (SNL) and Axel Kohlmeyer (Temple University).  Corresponding author:
Axel Kohlmeyer, akohlmey@gmail.com.

**Licensing**

By submitting this pull request, I agree, that my contribution will be included
in LAMMPS and redistributed under either the GNU General Public License version
2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

The analysis and the two code corrections in this pull request were produced by
Claude Opus 4.8 (Anthropic) under the direction of Joel Clemmer and Axel
Kohlmeyer, and reviewed by the directing authors.

**Backward Compatibility**

Results for `peri/eps` runs that reach the plastic regime will differ from
earlier versions (the model now yields at the intended stress and is stable);
purely elastic results are unaffected.  This is documented with a
`versionchanged` note in `doc/src/pair_peri.rst`.

**Implementation Notes**

Verified with an elastic-perfectly-plastic uniaxial test (incremental loading of
a periodic block): before the fix the deviatoric stress diverged/oscillated once
the plastic regime was entered; after the fix it yields at the intended stress
and plateaus stably.  The existing `PairPeri` unit test (`unittest/force-styles/
test_pair_peri.cpp`) still passes.

**Post Submission Checklist**

- [x] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [x] Suitable new documentation files and/or updates to the existing docs are included
- [x] The feature has been verified to work with the CMake based build system

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `697545ba8b25`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.