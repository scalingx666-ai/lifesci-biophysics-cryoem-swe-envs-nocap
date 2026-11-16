# Issue

**Title:** Created HDF5 based dataset

## Description

This is a rough draft of the HDF5 based dataset discussed in #214.  It can handle arbitrarily large datasets, even ones that are too large to fit in memory.  Are you interested in developing this into a supported feature?

## Description
I added a HDF5Dataset class that extends AtomicDataset.  It loads data from one or more HDF5 files with the format described at https://github.com/torchmd/torchmd-net/blob/main/torchmdnet/datasets/hdf.py.  The only data fields it currently supports are positions, atom types, energies, and forces.  Others could easily be added.

There's a lot that could be cleaned up.  I've only implemented the features that were needed for my own work.  That's especially true of `statistics()`, which only computes a subset of the available statistics.  It could be extended to compute the rest of them, but in the long term it might be better to rework the main statistics routine to support streaming calculations so it can work with any dataset class.

The class constructs AtomicData objects as needed in `get()`, which adds overhead.  You can partly compensate for that by increasing `dataloader_num_workers` to use multiple processes.

Computing statistics on large datasets can be slow.  I use `dataset_statistics_stride` to reduce the cost.

## Motivation and Context
All of the existing dataset classes require all the data to be held in memory at once.  This limits how much data can be used for training.
Resolves: #214.

## How Has This Been Tested?
I used it to train a model and it generally seems to work, but I haven't done extensive testing or written unit tests yet.

## Types of changes
<!-- What types of changes does your code introduce? Put an `x` in all the boxes that apply: -->
- [ ] Bug fix (non-breaking change which fixes an issue)
- [x] New feature (non-breaking change which adds or improves functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to change)
- [ ] Documentation improvement (updates to user guides, docstrings, or developer docs)

## Checklist:
<!-- Put an `x` in all the boxes that apply. If you're unsure about any of
     these, don't hesitate to ask. We're here to help! -->
- [x] My code follows the code style of this project and has been formatted using `black`.
- [x] All new and existing tests passed, including on GPU (if relevant).
- [x] I have added tests that cover my changes (if relevant).
- [ ] The option documentation (`docs/options`) has been updated with new or changed options.
- [ ] I have updated `CHANGELOG.md`.
- [ ] I have updated the documentation (if relevant).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `54805d243529`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.