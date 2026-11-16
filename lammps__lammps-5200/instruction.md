# Issue

**Title:** Sequential color map bugfix from PR #4577

## Description

**Summary**

This pull request contains only the bugfix for sequential color maps from PR #4577

**Author(s)**

Steve Plimpton

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

By submitting this pull request, I confirm that I did NOT use any AI tools to generate
all or parts of the code and modifications in this pull request.

**Backward Compatibility**

Color mapping is changed in the affected cases to the expected behavior.

**Post Submission Checklist**

- [x] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [x] Suitable new documentation files and/or updates to the existing docs are included
- [x] The added/updated documentation is integrated and tested with the documentation build system
- [x] The feature has been verified to work with the conventional build system
- [x] The feature has been verified to work with the CMake based build system
- [x] Suitable tests have been added to the unittest tree.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c8bd2ae5927e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.