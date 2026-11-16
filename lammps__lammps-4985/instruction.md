# Issue

**Title:** New Lennard-Jones soft core potential in FEP package

## Description

**Summary**

Addition of a new pair style (lj/cut/soft/gapsys) to the FEP package
The goal is to have a pair style implementing the 'Soft-core interactions: Gapsys et al.' functional form of GROMACS (https://manual.gromacs.org/current/reference-manual/functions/free-energy-interactions.html) in LAMMPS. The analytical form is described in V. Gapsys, D. Seeliger, and B.L. de Groot, “New Soft-Core Potential Function for Molecular Dynamics Based Alchemical Free Energy Calculations”, J. Chem. Theor. Comput., 8 2373-2382 (2012).

**Related Issue(s)**

No related issue

**Author(s)**

E. Voyiatzis at National Technical University of Athens , Greece ([evoyiatzis@gmail.com](mailto:evoyiatzis@gmail.com))

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

By submitting this pull request, I confirm that I did NOT use any AI tools to generate
all or parts of the code and modifications in this pull request.

**Backward Compatibility**

It does not break backward compatibility

**Implementation Notes**

<!--Provide any relevant details about how the changes are implemented, how correctness was verified, how other features - if any - in LAMMPS are affected-->

**Post Submission Checklist**

<!--Please check the fields below as they are completed **after** the pull request has been submitted. Delete lines that don't apply-->

- [ ] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [x] Suitable new documentation files and/or updates to the existing docs are included
- [ ] The added/updated documentation is integrated and tested with the documentation build system
- [ ] The feature has been verified to work with the conventional build system
- [ ] The feature has been verified to work with the CMake based build system
- [ ] Suitable tests have been added to the unittest tree.
- [ ] A package specific README file has been included or updated
- [ ] One or more example input decks are included

**Further Information, Files, and Links**

<!--Put any additional information here, attach relevant text or image files, and URLs to external sites (e.g. DOIs or webpages)-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7f680de2968e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.