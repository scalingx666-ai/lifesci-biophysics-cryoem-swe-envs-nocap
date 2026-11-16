# Issue

**Title:** Feedback review 1

## Description

A couple of bug fixes and improvements from our first feedback from reviewers, and from recent issues (#15 #21 #22)

### Added
- Improved the documentation on how to properly restrict interactions to ignore the protein backbone (Issue #22), how to fix the empty dataframe issue when no bond information is present in the PDB file (Issue #15), how to save the LigNetwork diagram (Issue #21), and some clarifications on using `fp.generate`
### Fixed
- Mixing residue type with interaction type in the interactive legend of the LigNetwork would incorrectly display/hide some residues on the canvas
- MOL2 files starting with a comment (`#`) would lead to an error

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3de6aa67899c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.