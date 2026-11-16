# Issue

**Title:** Fix netCDF file loading to no longer import scipy (and add netCDF to CI reqs)

## Description

A quick PR to address #1893 - changes below: 

1. Removes the line 

` netcdf = import_("scipy.io").netcdf_file `

from `formats/netcdf.py` which should fix netCDF4 loading issues.

2. Updated the `conda-envs/test.yaml` file to include netCDF4 in the requirements. This should ensure netCDF4 is in the CI env.

Let me know if I'm missing something!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f134696b3d6e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.