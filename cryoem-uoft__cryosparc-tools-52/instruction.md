# Issue

**Title:** Chore: Show path of dataset when exception encountered during loading

## Description

Easier to see which dataset load failed in an error traceback.

Example traceback:

```py
In [3]: d = Dataset.load("/Users/work/bench_big_dset_v2_old.cs")
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
File ~/Development/cryoem-uoft/cryosparc-tools/cryosparc/dataset.py:564, in Dataset.load(cls, file, cstrs)
    563 elif prefix != FORMAT_MAGIC_PREFIXES[CSDAT_FORMAT]:
--> 564     raise TypeError(f"Could not determine dataset format (prefix is {prefix})")
    566 headersize = u32intle(f.read(4))

TypeError: Could not determine dataset format (prefix is b'\x94CSDAT')

The above exception was the direct cause of the following exception:

DatasetLoadError                          Traceback (most recent call last)
Cell In[3], line 1
----> 1 d = Dataset.load("/Users/work/bench_big_dset_v2_old.cs")

File ~/Development/cryoem-uoft/cryosparc-tools/cryosparc/dataset.py:597, in Dataset.load(cls, file, cstrs)
    594         return dset
    596 except Exception as err:
--> 597     raise DatasetLoadError(f"Could not load dataset from file {file}") from err

DatasetLoadError: Could not load dataset from file /Users/work/bench_big_dset_v2_old.cs
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2d16e9ffa00e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.