# Issue

**Title:** PLUMED/SCAFACOS: fixed typos to make it MPI_<LANG>_COMPILER

## Description

**Summary**

fixed typos to make it MPI_<LANG>_COMPILER instead of CMAKE_MPI_<LANG>_COMPILER.

**Related Issue(s)**

<!--If this addresses an open GitHub issue for this project, please mention the issue number here, and describe the relation. Use the phrases `` or ``, when you want an issue to be automatically closed when the pull request is merged-->

**Author(s)**

Saswat Mishra, University of South Florida - sswtmshr@gmail.com

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

Used Anthropic Claude to generate  the reproducer for the PR (check implementation notes)

**Backward Compatibility**

<!--Please state whether any changes in the pull request will break backward compatibility for inputs, and - if yes - explain what has been changed and why-->
Breaks nothing. Silently falls back to whatever PLUMED picks up. I noticed it because it ended up with a mixed toolchain for my build.

**Implementation Notes**

Was building plumed -> noticed mixed toolchain for C and C++.  plumed had empty strings for C and CXX and silently picked up mixed from the path.
Found the typo in plumed cmake, grepped the typo and found it in scafacos as well. 

Reproducer:
```cmake
cmake_minimum_required(VERSION 3.20)
project(mpivar C CXX)
find_package(MPI REQUIRED)
message(STATUS "MPI_CXX_COMPILER       = '${MPI_CXX_COMPILER}'")
message(STATUS "CMAKE_MPI_CXX_COMPILER = '${CMAKE_MPI_CXX_COMPILER}'")
```
Result for me in NERSC Perlmutter:
```
-- MPI_CXX_COMPILER       = '/opt/cray/pe/craype/2.7.36/bin/CC'
-- CMAKE_MPI_CXX_COMPILER = ''
```

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

**Further Information, Files, and Links**

<!--Put any additional information here, attach relevant text or image files, and URLs to external sites (e.g. DOIs or webpages)-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c8bd2ae5927e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.