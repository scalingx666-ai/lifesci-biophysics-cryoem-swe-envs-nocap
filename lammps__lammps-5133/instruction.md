# Issue

**Title:** Collected small changes and fixes

## Description

**Summary**

This pull request contains multiple small changes and fixes

**Related Issue(s)**

N/A

**Author(s)**

Aidan Thompson, SNL

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

By submitting this pull request, I confirm that I did NOT use any AI tools to generate
all or parts of the code and modifications in this pull request.

**Backward Compatibility**

N/A

**Implementation Notes**

The following individual changes are included:

- Added Nick Hagerty to TSC (this should also get propagated to lammps.org)
- Tweak unit test epsilon to pass tests on ARM64

**Post Submission Checklist**

- [ ] The feature or features in this pull request is complete
- [ ] Licensing information is complete
- [ ] Corresponding author information is complete
- [ ] The source code follows the LAMMPS formatting guidelines
- [ ] Suitable new documentation files and/or updates to the existing docs are included
- [ ] The added/updated documentation is integrated and tested with the documentation build system
- [ ] The feature has been verified to work with the conventional build system
- [ ] The feature has been verified to work with the CMake based build system
- [ ] Suitable tests have been added to the unittest tree.
- [ ] A package specific README file has been included or updated
- [ ] One or more example input decks are included

**Further Information, Files, and Links**

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `38fe46820cb1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.