# Issue

**Title:** Boundaries includable in lb and ek vtk

## Description

Description of changes:
- The vtk data inside of boundaries cells can be written by toggeling the flag include_boundaries in the vtk_wrtier class.
- Boundaries can also be tracked as an observable by the vtk writers

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `55c413177680`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.