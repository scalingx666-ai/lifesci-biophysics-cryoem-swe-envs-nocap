# Issue

**Title:** In cftbx, handle more recent versions of INTEGRATE.HKL

## Description

Current versions of XDS produce an `INTEGRATE.HKL` with this line:

```
!ORGX=   1202.11  ORGY=   1213.24  DETECTOR_DISTANCE=   213.027
```

whereas old versions had these items on separate lines:
```
!ORGX=   1202.11  ORGY=   1213.24
!DETECTOR_DISTANCE=   213.027
```
This PR updates cftbx to handle both the new and old cases, without it an assertion is raised due to the detector distance not being found.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b737ac62405e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.