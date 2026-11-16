# Issue

**Title:** MBX v1.4.0

## Description

**Summary**

Updated the MBX package TAR to v1.4.0. TAR contains performance improvements due to better SIMD vectorization.

**Author(s)**

Henry Agnew, UC San Diego.

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

By submitting this pull request, I confirm that I did NOT use any AI tools to generate
all or parts of the code and modifications in this pull request.

**Implementation Notes**

<!--Provide any relevant details about how the changes are implemented, how correctness was verified, how other features - if any - in LAMMPS are affected-->

Compiled MBX+LAMMPS and verified that the output matches the expected example output. Afterwards, I updated the example timings due to the speed improvements.

**Post Submission Checklist**

<!--Please check the fields below as they are completed **after** the pull request has been submitted. Delete lines that don't apply-->

- [X] The feature or features in this pull request is complete
- [X] Licensing information is complete
- [X] Corresponding author information is complete
- [X] The source code follows the LAMMPS formatting guidelines
- [ ] Suitable new documentation files and/or updates to the existing docs are included
- [ ] The added/updated documentation is integrated and tested with the documentation build system
- [ ] The feature has been verified to work with the conventional build system
- [X] The feature has been verified to work with the CMake based build system
- [ ] Suitable tests have been added to the unittest tree.
- [ ] A package specific README file has been included or updated
- [X] One or more example input decks are included

**Further Information, Files, and Links**

Technical details of the improvements in MBX v1.4 are documented in our preprint: https://chemrxiv.org/doi/full/10.26434/chemrxiv.15005580/v1

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4817447e61d7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.