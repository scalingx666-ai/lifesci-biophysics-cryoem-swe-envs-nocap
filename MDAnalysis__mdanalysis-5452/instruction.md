# Issue

**Title:** Rotate velocities and forces besides positions during trajectory rotation

## Description

<!-- 
Insert issue number that this PR fixes (if any) just after 'Fixes #'.
If this PR does not fix an existing issue, consider opening one or remove 'Fixes #' from the PR description.
-->


Changes made in this Pull Request:
<!-- Describe the changes that this PR makes. If applicable, use the following bullet list. --> 
- **Modified `AtomGroup.rotate()`** in `package/MDAnalysis/core/groups.py` to automatically rotate velocities and forces in-place alongside positions when the data arrays are present in the trajectory (`ts.has_velocities` and `ts.has_forces`).
- This also affects `MDAnalysis.analysis.align.alignto()` and `AlignTraj`, since they
   apply their fit via `AtomGroup.rotate()`. 
- **Note:** this changes existing behavior, as velocities/forces were previously left untouched by
   rotation.
- **Added comprehensive test suites** across three test modules:
  * `MDAnalysisTests/core/test_atomgroup.py` (`test_rotate_velocities_forces` and `test_rotate_no_velocities_forces_does_not_raise`)
  * `MDAnalysisTests/analysis/test_align.py` (`test_alignto_rotates_velocities_and_forces`)
  * `MDAnalysisTests/transformations/test_rotate.py` (`test_rotateby_velocities_forces` and `test_rotateby_no_velocities_forces_does_not_raise`)

## LLM / AI generated code disclosure
<!-- Please update this disclosure to reflect if you did or did not use LLMs / AI to generate code -->
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: no

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [x] Issue raised/referenced?
 - [x] Tests updated/added?
 - [x] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)
 - [x] LLM/AI disclosure was updated.

## Developers Certificate of Origin
<!-- 
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `83bc9a238bbf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.