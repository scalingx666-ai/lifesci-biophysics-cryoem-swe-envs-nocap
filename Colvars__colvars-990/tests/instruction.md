# Issue

**Title:** ci: try using apptainer instead of apptainer-suid

## Description

## Summary

It looks like `apptainer-suid` is not in the ppa repository anymore, so I change it to `apptainer`.

## Related issue(s)

N/A

## Affected engine(s)

CI only

## Author(s)

@HanatoK

## Licensing

By submitting this pull request, I agree that my contribution will be included in Colvars repository and redistributed under the GNU Lesser General Public License (LGPL) version 3.

*Note:* NAMD, VMD and TinkerHP follow more restrictive licenses, e.g. the [UIUC license](https://www.ks.uiuc.edu/Research/namd/license.html).  However, the terms of the LGPLv3 remain in effect for the copies of the Colvars code distributed alongside those packages.

## Artificial Intelligence (AI)

No.

## Further information / links

N/A

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3d783854b174`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.