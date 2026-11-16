# Issue

**Title:** Avoid multiple processes reading from the same HDF5 file

## Description

When using the HDF5 dataset class we sometimes run into multiprocessing issues where multiple workers read from the same h5py file instance, which might corrupt the data (discovered in #82). Reading from one h5py file instance using multiple processes is not recommended according to [their documentation](https://docs.h5py.org/en/stable/mpi.html). This PR avoids the problem by initializing the h5py files once in the dataset's `get` method instead of the constructor.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8e734ebde3ff`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.