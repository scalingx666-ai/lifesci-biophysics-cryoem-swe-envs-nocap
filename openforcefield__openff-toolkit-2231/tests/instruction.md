# Issue

**Title:** Stop OpenEyeToolkitWrapper from erroring on planar trivalent nitrogens if they have trivalent carbon neighbors

## Description

- [x] Makes `OpenEyeToolkitWrapper.from_openeye` not raise an undefined-stereo error/warning for planar Ns if they have at least one trivalent carbon neighbor (This isn't a perfect solution but does cover a large fraction of the annoying over-policing of nitrogen stereo by OETKW)
- [x]  
- [x]  
- [x]  
- [x] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [x] [Lint](https://docs.openforcefield.org/projects/toolkit/en/stable/users/developing.html#style-guide) codebase
- [x] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b3ad73d58d25`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.