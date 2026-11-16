# Issue

**Title:** Require an argument for the start of the diffusive regime.

## Description

This is a **breaking change**, hence the increase in minor version number. 

This will mean that the `diffusion` method will take a required argument that is the start of the diffusive regime in the simulation (where the analysis of the diffusion relation will begin). 

Therefore, if you identify diffusion as starting at 4 ps (see [notebooks](https://kinisi.readthedocs.io/en/latest/vasp_d.html) for examples), the following is required:

```
diff.diffusion(4)
```

Any feedback is welcome.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7d62dfb52402`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.