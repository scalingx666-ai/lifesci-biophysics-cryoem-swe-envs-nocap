# Issue

**Title:** build: fix the HIP compilation with ROCm 6.x and gfx1030

## Description

## Summary

Some targets like gfx1030, only supports a wavefront size of 32 by default, which makes the device code fail to compile on ROCm 6.x. This commit aims to solve the issue by checking `__AMDGCN_WAVEFRONT_SIZE__`. Since ROCm 7.x, I think the issue of HIP cooperative_groups has been solved and this workaround is not required anymore.

## Related issue(s)

N/A

## Affected engine(s)

NAMD

## Author(s)

@HanatoK 

## Licensing

By submitting this pull request, I agree that my contribution will be included in Colvars repository and redistributed under the GNU Lesser General Public License (LGPL) version 3.

*Note:* NAMD, VMD and TinkerHP follow more restrictive licenses, e.g. the [UIUC license](https://www.ks.uiuc.edu/Research/namd/license.html).  However, the terms of the LGPLv3 remain in effect for the copies of the Colvars code distributed alongside those packages.

## Artificial Intelligence (AI)

N/A

## Further information / links

<!--For anything relevant that is not covered by the above, feel free to add links or attachments here.-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e5cddcf29d30`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.