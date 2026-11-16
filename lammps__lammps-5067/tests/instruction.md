# Issue

**Title:** Fix ttm cascade

## Description

**Summary**

Addition of a new fix style (ttm/cascade) to the EXTRA-FIX package. The new fix extends fix ttm/grid and enables users to: (i) select two different schemes for the treatment of electron–ion interactions during the early stages of radiation damage cascade simulations, and (ii) provide table files to account for variations in the electronic specific heat and electronic thermal conductivity with the local electronic temperature. A more complete description can be found in the documentation or in the corresponding publication.

**Author(s)**

Andres Rojano, Lancaster University (aerc9108@gmail.com)
Samuel T Murphy, Lancaster University

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

By submitting this pull request, I confirm that I did NOT use any AI tools to generate
all or parts of the code and modifications in this pull request.

**Backward Compatibility**

No impact.

**Implementation Notes**

Core functionality remains largely the same as ttm/grid, which was modified to allow inheritance. The changes focus on the treatment of electron–ion interactions and the inclusion of variations in the electronic specific heat and electronic thermal conductivity with the local electronic temperature. No further impact on other parts of LAMMPS.

**Post Submission Checklist**

<!--Please check the fields below as they are completed **after** the pull request has been submitted. Delete lines that don't apply-->

- [ ] The feature or features in this pull request is complete
- [ ] Licensing information is complete
- [ ] Corresponding author information is complete
- [ ] The source code follows the LAMMPS formatting guidelines
- [ ] Suitable new documentation files and/or updates to the existing docs are included
- [ ] The added/updated documentation is integrated and tested with the documentation build system
- [ ] The feature has been verified to work with the conventional build system
- [ ] The feature has been verified to work with the CMake based build system
- [ ] Suitable tests have been added to the unittest tree.
- [ ] One or more example input decks are included

**Further Information, Files, and Links**

Publication link : https://dx.doi.org/10.1088/1361-648X/ad4941

The following link contains a GitHub repository with validation tests for the FixTTMCascade implementation: https://github.com/andresrojanoc/FixTTMCascade-Tests

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4e5eb25ab6e9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.