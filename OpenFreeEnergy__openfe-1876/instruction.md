# Issue

**Title:** Minor fix to support ABFE gathering with openfe v1.9+

## Description

Small update to gather machinery to avoid picking results from UnitResults that aren't the Analysis unit.
It's not the nicest fix, but it works in a backwards compatible manner.

## Developers certificate of origin
- [x] I certify that this contribution is covered by the MIT License [here](https://github.com/OpenFreeEnergy/openfe/blob/main/LICENSE) and the **Developer Certificate of Origin** at <https://developercertificate.org/>.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `be08a6224595`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.