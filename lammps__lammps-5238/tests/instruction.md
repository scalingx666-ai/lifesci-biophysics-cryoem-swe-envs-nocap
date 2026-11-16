# Issue

**Title:** fix srd: corrections to boundary bin velocity shift

## Description

**Summary**

Follow-up bugfix to #4590. The velocity shift that `fix srd` applies to SRD particles in the last (periodic-boundary) collision bins was not undone or handled correctly in three cases:

1. Orthogonal boxes with a size-, not shape-changing `fix deform ... remap v` together with `tstat no` or `unbiased yes`. The undo loop applied x2lamda, whereas bin size and corner were in box units.
2. For the profile-biased thermostat (the original variant, `unbiased no`), bins with only a single particle were handled incorrectly. They are skipped by the thermostat, and the shift needs to be undone.
3. The boundary bin velocity shift was applied even for a `fix deform` that only changed the box size (with `remap x` or `none`).

**Related Issue(s)**

Related to #4590 and #4602.

**Author(s)**

Victor Taenzel, University of Freiburg

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

By submitting this pull request, I confirm that I did NOT use any AI tools to generate
all or parts of the code and modifications in this pull request.

**Backward Compatibility**

Input does not change, and only faulty results are fixed.

**Implementation Notes**

I ran the same tests as for #4590 , in serial and with mpi. On top, I tested the flawed cases explained above.

**Post Submission Checklist**

- [ ] The feature or features in this pull request is complete
- [ ] Licensing information is complete
- [ ] Corresponding author information is complete
- [ ] The source code follows the LAMMPS formatting guidelines
- [ ] The feature has been verified to work with the conventional build system
- [ ] The feature has been verified to work with the CMake based build system

**Further Information, Files, and Links**

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a0e2bd6efb3d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.