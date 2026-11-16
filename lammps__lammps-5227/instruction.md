# Issue

**Title:** balance: shift procgrid

## Description

**Summary**

Balance: reallocate SHIFT work arrays when the processor grid changes

Balance::shift_setup_static() sizes onecost/allcost/sum/target/lo/hi/losum/hisum from max(comm->procgrid) once.
any resizing later leaads to heap overflow -> crash or hang ( aborts with glibc heap-corruption error or 1 rank spinning in utils::binary_search)

The fix adds `Balance::shift_grow()`, which reallocates the vectors when `max(procgrid)` exceeds the allocated length. It is called from `shift_setup_static()` and at the start of `shift()`.

Tested on frontier.

**Related Issue(s)**

**Author(s)**
Saswat Mishra, University of South Florida - sswtmshr@gmail.com

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

I used Anthropic Claude Opus 5.5 to A/B test the bugfix and also helped with the diagnosis.

**Backward Compatibility**

No backward compatibility issue. 

**Implementation Notes**

`shift_grow()` is a wrapper around existing logic. used a function because it will be called twice.
The fix adds `Balance::shift_grow()`, which reallocates the vectors when `max(procgrid)` exceeds the allocated length. It is called from `shift_setup_static()` and at the start of `shift()`.


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
- [ ] A package specific README file has been included or updated
- [ ] One or more example input decks are included

**Further Information, Files, and Links**

<!--Put any additional information here, attach relevant text or image files, and URLs to external sites (e.g. DOIs or webpages)-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e891a3e10973`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.