# Issue

**Title:** Decouple vdW and virtual site parameters

## Description

The current structure of `VirtualSiteHandler.VirtualSiteType` couples together electrostatics, geometry, and (12-6) vdW interactions. This forces unnecessary copying of hundreds of lines of code if anybody wishes to build off of this class using non-standard functional forms.

This change refactors `VirtualSiteType` to enable this downstream use case without changing the public API or behavior.

In the future it may be worth moving the charge increment logic from `_BasicVirtualSiteType` to `VirtualSiteType`; this change neither does that nor prevents it from happening in the future.

Credit to @jthorton who came up with this idea and implemented it; I just plucked it over here since it doesn't make sense for downstream packages to re-implement toolkit offerings

- [ ] Tag issue being addressed
- [x] De-couple non-bonded and virtual site parameters
- [x] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [x] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://open-forcefield-toolkit.readthedocs.io/en/latest/developing.html#style-guide) codebase
- [ ] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.rst)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c00d7a7d74d5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.