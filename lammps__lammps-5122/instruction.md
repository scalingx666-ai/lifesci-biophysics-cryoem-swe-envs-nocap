# Issue

**Title:** Bug fix for lj/cut/tip4p/long/gpu with pair hybrid

## Description

**Summary**

This PR aims to fix the bug reported by #5121 for pair style `lj/cut/tip4p/long/gpu` when used with `pair hybrid`.

**Related Issue(s)**

#5121 

**Author(s)**

Trung Nguyen (U Chicago)

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

Anthropic Claude Opus 5 is used and supervised by the PR author for the bug fix.

**Backward Compatibility**

Should maintain backward compatibility.

**Implementation Notes**

The analysis given by the reporter in issue #5121 was used as input for the AI coding agent. The input deck provided therein was then used for validation of the changes in the PR. The new data structure `nbor_row` is allocated to `nall` and used to map the actual atom index to the neighbor list row. This is to ensure the indexing is correct for skipped entries in the neigh lists. The stride of `nall` is then used for indexing `ans0` and `nbor_row`.

**Post Submission Checklist**

<!--Please check the fields below as they are completed **after** the pull request has been submitted. Delete lines that don't apply-->

- [x] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [ ] Suitable new documentation files and/or updates to the existing docs are included
- [ ] The added/updated documentation is integrated and tested with the documentation build system
- [ ] The feature has been verified to work with the conventional build system
- [x] The feature has been verified to work with the CMake based build system
- [x] Suitable tests have been added to the unittest tree.
- [ ] A package specific README file has been included or updated
- [ ] One or more example input decks are included

**Further Information, Files, and Links**

<!--Put any additional information here, attach relevant text or image files, and URLs to external sites (e.g. DOIs or webpages)-->

A new unit test for tip4p and pair hybrid is added: `unittests/force-styles/tests/mol-pair-hybrid_tip4p_long.yaml`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f0342f1f0176`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.