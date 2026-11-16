# Issue

**Title:** Collected small fixes and improvements

## Description

**Summary**

This pull request contains multiple small fixes and improvements

**Related Issue(s)**

N/A

**Author(s)**

Axel Kohlmeyer, Temple U

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

Most of the programming was done by Claude code (Fable 5) under the direct supervision and with specific instructions from a human. All changes were reviewed and are confirmed.

**Backward Compatibility**

N/A

**Implementation Notes**

The following individual changes are included:
- Prevent ELECTRODE package `fix electrode/*` fixes to be (incorrectly) used with run-style respa
- add regression test for pair style `lj/long/dipole/long`. Must use the fix-timestep harness since the pair style harness does not track torques.
- propagate accumulated agentic coding lessons from the last 4 weeks to bundled agent instructions documents
- add porting and auditing guide for the OPENMP package, similar to the KOKKOS instructions already available
- fix memory leak in pair style ldd when using pair_coeff multiple times
- avoid reading uninitialized id3p/id5p in pair_coeff with pair style oxdna2/dh

**Post Submission Checklist**

- [ ] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [ ] Suitable new documentation files and/or updates to the existing docs are included
- [ ] The added/updated documentation is integrated and tested with the documentation build system
- [ ] The feature has been verified to work with the conventional build system
- [x] The feature has been verified to work with the CMake based build system
- [x] Suitable tests have been added to the unittest tree.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6404ba2eac4c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.