# Issue

**Title:** Add BAOAB Langevin integrator fix

## Description

**Summary**

Implements the BAOAB splitting integrator of Leimkuhler & Matthews (2013) as a standalone fix (fix baoab). Achieves O(dt^2) configurational accuracy without a separate fix nve.

- src/EXTRA-FIX/: fix_baoab.cpp/h
- doc/src/fix_baoab.rst: full fix documentation
- doc/src/Commands_fix.rst, Packages.rst, Packages_details.rst: index entries
- examples/PACKAGES/baoab/: LJ fluid NVT example

**Related Issue(s)**

This was discussed on https://matsci.org/t/baoab-integrator/67078/

**Author(s)**

- [ ] add contribution author details as explained in comment below

<!--Please state name and affiliation of the author or authors that should be credited with the changes in this pull request. If this pull request adds new files to the distribution, please also provide a suitable "long-lived" e-mail address (ideally something that can outlive your institution's e-mail, in case you change jobs) for the *corresponding* author, i.e. the person the LAMMPS developers can contact directly with questions and requests related to maintenance and support of this contributed code.-->

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

By submitting this pull request, I confirm that I did NOT use any AI tools to generate
all or parts of the code and modifications in this pull request.

**Backward Compatibility**

N/A

**Implementation Notes**

N/A

**Post Submission Checklist**

- [x] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [x] Suitable new documentation files and/or updates to the existing docs are included
- [x] The added/updated documentation is integrated and tested with the documentation build system
- [x] The feature has been verified to work with the conventional build system
- [x] The feature has been verified to work with the CMake based build system
- [ ] Suitable tests have been added to the unittest tree.
- [ ] A package specific README file has been included or updated
- [x] One or more example input decks are included

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `704e0160b4e3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.