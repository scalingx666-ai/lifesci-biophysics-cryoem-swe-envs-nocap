# Issue

**Title:** Allow appending to HDF5 files using `trajectory.save_hdf5`

## Description

Addressing #1750 (and maybe #1843) - this adds an additional argument in `core/trajectory.save_hdf5()` called `savemode` that allows you to define either "w" or "a" for write or append mode respectively. I've set it to `savemode="w"` by default.

The added docstring reads
```
        savemode : str, default='w'
            The mode in which to save the file. 'w' will overwrite any existing
            file, 'a' will append to an existing file.
```

I've also added a snipped to check that only "w" or "a" are being passed and raise a `ValueError` otherwise.

It's worth noting that `HDF5TrajectoryFile` objects already accept "a" as a valid mode as shown in two portions of the `__init__` - [spot 1](https://github.com/mdtraj/mdtraj/blob/15514a57a98bebb3db6f6b6fdc3832f06a6197df/mdtraj/formats/hdf5.py#L183-L184) and [spot 2](https://github.com/mdtraj/mdtraj/blob/15514a57a98bebb3db6f6b6fdc3832f06a6197df/mdtraj/formats/hdf5.py#L210-L216)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `15514a57a98b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.