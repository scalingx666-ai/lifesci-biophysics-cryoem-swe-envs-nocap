# Issue

**Title:** Collected small changes and fixes

## Description

**Summary**

This pull request contains multiple small changes and fixes

**Related Issue(s)**

N/A

**Author(s)**

Axel Kohlmeyer, Temple U

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

The fix for the file access race in the dump vtk unit test was created by Claude Code (Opus 5.5) on my behalf.

**Backward Compatibility**

N/A

**Implementation Notes**

The following changes are included:
- fix a file access race in the unit test for dump vtk
- apply small coding style changes suggested by static code analysis
- remove GitHub workflows that are now served by https://ci.lammps.org
- apply some of the remaining check also to pull requests using the free capacity

**Post Submission Checklist**

- [ ] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] AI tools attribution is included
- [x] The source code follows the LAMMPS formatting guidelines
- [ ] Suitable new documentation files and/or updates to the existing docs are included
- [ ] The added/updated documentation is integrated and tested with the documentation build system
- [ ] The feature has been verified to work with the conventional build system
- [x] The feature has been verified to work with the CMake based build system
- [x] Suitable tests have been added to the unittest tree.
- [ ] A package specific README file has been included or updated
- [ ] One or more example input decks are included

**Further Information, Files, and Links**

https://ci.lammps.org/

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `823eafc0a855`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.