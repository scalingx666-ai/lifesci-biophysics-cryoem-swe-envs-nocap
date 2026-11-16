# Issue

**Title:** Set the warning type when making bonds does not find a block

## Description

When the nake_bonds processor does not find a block, it sends the
following warning:

```
WARNING - general - Can't add bonds based on atom names for residue F-GDP501 because 'Residue GDP is not known to force field universal'. Falling back to distance criteria.
```

This commit sets the warning type to `unknown-residue` so if
`unknown-residue` is ignored, that warning is ignored.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8801913d3bc9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.