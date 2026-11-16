# Issue

**Title:** Sets netCDF4 as default in NetCDFTrajectoryFile, fallback to scipy

## Description

This PR addresses #1831.

I've adopted the strategy suggested by the Issue author, based on an approach taken in ParmEd and MDAnalysis, to use netCDF4 if installed and fall back to SciPy netCDF if not installed.

I based the code for netCDF4 on the original version in the code base. It used netCDF4 before SciPy netCDF, PR that changed it here - 

Here is a small demo notebook that demonstrates both backends being used: https://github.com/janash/mdtraj-netcdf/blob/main/openmm.ipynb

I also updated the message about `netCDF4` to give information about `conda` and have an updated URL

One thing I'm not quite sure about is using `import_`. It gives a very verbose message, but it's not actually causing an Exception with my current use, so may be confusing.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `da52c9c364e6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.