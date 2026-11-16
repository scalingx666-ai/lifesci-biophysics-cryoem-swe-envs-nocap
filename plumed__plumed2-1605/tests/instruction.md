# Issue

**Title:** Reading/writing hdf5 files from python

## Description

Implemented the possibility to read and write plumed hdf5 files from python.

```
  import plumed
  df = plumed.read_as_pandas("COLVAR.h5")
  # Modify df
  plumed.write_pandas(df, "COLVAR_NEW.h5")
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e0e36e2934c6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.