# Issue

**Title:** Last minute bug fixes and updates for the first stable release update

## Description

**Summary**

This pull request contains a few final updates and bug fixes for the stable release update.

**Related Issue(s)**

N/A

**Author(s)**

Axel Kohlmeyer, Temple U

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

The ReaxFF memory issue and bugfix was fixed by Claude code (Opus 5.5) with my oversight.
The rest of the changes were made without AI assistance.

**Backward Compatibility**

N/A

**Implementation Notes**

The following changes are included:
- set LAMMPS-GUI version to include to v3.1.2
- reverse ordering of allocation of new and release of old buffers in QEq for ReaxFF and thus cutting the memory requirements roughly in half.
- honor the -nonbuf/-nb command line flag also without using partitions
- backport PR #5276 

**Post Submission Checklist**

- [x] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] AI tools attribution is included
- [x] The source code follows the LAMMPS formatting guidelines
- [ ] Suitable new documentation files and/or updates to the existing docs are included
- [ ] The added/updated documentation is integrated and tested with the documentation build system
- [ ] The feature has been verified to work with the conventional build system
- [x] The feature has been verified to work with the CMake based build system
- [ ] Suitable tests have been added to the unittest tree.
- [ ] A package specific README file has been included or updated
- [ ] One or more example input decks are included

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4f20d4f8266e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.