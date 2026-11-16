# Issue

**Title:** fix bond/react: send vs recv mpi bug

## Description

**Summary**

Before bugfix: could be delay in reactions in parallel unless at least one reaction was straddling procs or on rank 0

**Related Issue(s)**

none

**Author(s)**

Jake

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

Opus 5.5 found this one

By submitting this pull request, I confirm that I did NOT use any AI tools to generate
all or parts of the code and modifications in this pull request.

**Backward Compatibility**

<!--Please state whether any changes in the pull request will break backward compatibility for inputs, and - if yes - explain what has been changed and why-->

**Implementation Notes**

<!--Provide any relevant details about how the changes are implemented, how correctness was verified, how other features - if any - in LAMMPS are affected-->

**Post Submission Checklist**

<!--Please check the fields below as they are completed **after** the pull request has been submitted. Delete lines that don't apply-->

- [x] The feature or features in this pull request is complete
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

<!--Put any additional information here, attach relevant text or image files, and URLs to external sites (e.g. DOIs or webpages)-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ec02ed0f8b0b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.