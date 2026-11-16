# Issue

**Title:** Mock-testing dependency absence for netCDF4

## Description

As discussed in #1893 and #1894, I mocked patched netcdf4 off. I used ``pytest`` instead of ``unittest.mock`` since I'm more familiar with that.

Also added ambertools into the dev env to run the cpptraj test.  I believe it was added to conda-forge 1-2 years ago.


Some reading/reference materials I used in this:
https://stackoverflow.com/questions/5386194/simulating-lack-of-a-dependency-when-testing-a-python-script
https://stackoverflow.com/a/42334155/17047865


NOTE: #1894 should be merged in first.

EDIT: modified two trr tests so they could run out-of-order and still succeed. (https://github.com/mdtraj/mdtraj/actions/runs/9358413371/job/25760203051#step:6:2394)

EDIT: Added some more warnings to ignore (inherit to the testing). That should clean up the CI output for better readability.

EDIT: Fixed a Numpy 1.25 deprecation regarding casting n-d arrays (ndim> 0) to a scalar (e.g., `float(np.ndarray([[[3]]]))` to `float(3)`):    https://numpy.org/devdocs/release/1.25.0-notes.html#deprecations

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `15514a57a98b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.