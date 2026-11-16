# Issue

**Title:** Add SPICE 2.0.1, Fix bug in MemmappedDataset

## Description

Adds compatibility for the latest SPICE version https://arxiv.org/pdf/2406.13112

In doing so I uncovered a couple of issues related to MemmapedDataset.
### I fixed MemmapedDataset overwriting the names of processed datasets.
 In particular, SPICE sets self.name as:
```python
        arg_hash = f"{version}{subsets}{max_gradient}{subsample_molecules}"
        arg_hash = hashlib.md5(arg_hash.encode()).hexdigest()
        self.name = f"{self.__class__.__name__}-{arg_hash}"
``` 
Which gets overwritten in the MemmappedDataset constructor:
```python
self.name = self.__class__.__name__
```
Causing any subsample/gradient configurations to be stored as just "SPICE.*.mmap". Probably ignoring subsample/gradient if the dataset has been already processed.

### This in turn uncovered a nasty bug in the test for the ACE Dataset. 
Since all instances of the Dataset are named with just `self.__class__.__name__`, creating two instances of ACE will simply ignore the data in the second and use what was processed in the first to a group of files just called "Ace.*.mmap".
This test should not pass but it was passing until now:
https://github.com/torchmd/torchmd-net/blob/e908988440f2e9d77029a15c41fcee8139ee8b7f/tests/test_datasets.py#L246-L299
In Ace v2 the field is called `dipole_moments`. But MemmappedDataset was not even trying to process the second one.

### I also made some changes
- Now SPICE checks the hash of the downloaded files.
- Now SPICE stores files under `{root}/raw/spice/` or `{root}/processed/spice`. Before it was storing things under `{root}/raw/{version}`, which could collide with other datasets. 

**Note that this PR will invalidate many preprocessed datasets, prompting for redownloading and reprocessing**. I recommend you delete the dataset storage folder and let it redownload things.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e908988440f2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.