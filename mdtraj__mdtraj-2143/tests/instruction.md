# Issue

**Title:** itertool fix for libnetcdf4 >= 4.10.0

## Description

Problem:
`netCDF4` now throws `IndexError` when indexing ranges outside the dataset with `libnetcdf >=4.10.0`. It previously just ignores those indices with `libnetcdf <= 4.9.3`.

`iterload` was previously a little lax in determining the frame slice, especially when the total number of frames do not divide neatly by the stride. This PR introduces a guard to calculate exactly what indices we need. Fix is backwards compatible.


Example:
Minimal reproducible example with libnetcdf 4.10.0. Note that frame0.nc has 501 frames so the slice indexed out of the shape. If you don't have the `tests/data/` folder, manually untar `data.tar.gz` or just run any one of the tests with pytest.
```
import netCDF4
with netCDF4.Dataset('tests/data/frame0.nc', format='netCDF3') as file:
    coord = file.variables['coordinates'][slice(500, 510,2)]
```

Error:
```
---------------------------------------------------------------------------
IndexError                                Traceback (most recent call last)
Cell In[4], line 2
      1 with netCDF4.Dataset('tests/data/frame0.nc', format='netCDF3') as file:
----> 2     coord = file.variables['coordinates'][slice(500, 510,2)]

File src/netCDF4/_netCDF4.pyx:5107, in netCDF4._netCDF4.Variable.__getitem__()

File src/netCDF4/_netCDF4.pyx:6079, in netCDF4._netCDF4.Variable._get()

IndexError: index exceeds dimension bounds

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d0ab65fa9e8e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.