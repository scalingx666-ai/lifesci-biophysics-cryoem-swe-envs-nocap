# Issue

**Title:** Speed up automatic determination of PPPM grid for pppm/dipole (and pppm)

## Description

**Summary**

This pull request significantly reduces the time it takes for kspace style `pppm/dipole` to determine the FFT grid compatible with the required accuracy. A smaller part of the improvement should also apply to all pppm styles.

**Related Issue(s)**



**Author(s)**

Axel Kohlmeyer, Temple U

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

Claude Code (Opus 5.5) detected the issue and suggested the improvements. Available unit tests confirm that the results are unchanged.

**Backward Compatibility**

Yes.

**Implementation Notes**

Please see the individual commit messages for details of what is changed and how.

**Post Submission Checklist**

- [x] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] AI tools attribution is included
- [x] The source code follows the LAMMPS formatting guidelines system
- [x] The feature has been verified to work with the conventional build system
- [x] The feature has been verified to work with the CMake based build system

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `503edac323d1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.