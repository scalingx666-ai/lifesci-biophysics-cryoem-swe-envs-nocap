# Issue

**Title:** Refactor Variable class storage management

## Description

**Summary**

This pull request replaces the multiple managed lists with per-variable data with an STL vector container of a new VarInfo struct.  This delegates the memory management to the container and allows to simplify the code for creating and updating variables. The access to variables for the rest of LAMMPS is mostly unchanged. Only some low level access from the Info class or the C-language library interface required some small adjustments.  Some unnecessary variable lookups from thermostats were removed.

**Related Issue(s)**

As discussed with @sjplimp 

**Author(s)**

Axel Kohlmeyer, Temple U

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

The first draft of this refactor was created by Google Gemini 3.

**Backward Compatibility**

Yes. Some error messages were improved.

**Post Submission Checklist**

- [x] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [x] The feature has been verified to work with the conventional build system
- [x] The feature has been verified to work with the CMake based build system
- [x] Suitable tests have been updates in the unittest tree.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7f680de2968e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.