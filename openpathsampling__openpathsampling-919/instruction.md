# Issue

**Title:** use uuid instead of 'is' for selecting ensembles in rate calculation

## Description

This came up on experimental storage, but should also be a bug on `master`.

If you load your `network` from one `Storage object`, but your `steps` from another (loading the same file), you might end up with 2 different `instances` of the same `Ensemble`. 
The `is` evaluation is then too strict to handle this. 

I opted to convert this to an `uuid == uuid`

I am having trouble setting up a test case for this, so if you have any pointers for that @dwhswenson, please let me know.

I would envision something like this should fail on master (but I don't have any workable `.nc` files at the moment, and couldn't quickly spot one in the test directory)
```python
import openpathsampling as paths

s1 = paths.Storage("test.nc", 'r')
s2 = paths.Storage("test.nc", 'r')

network = s1.networks[0]

network.hist_args['max_lambda'] = { 'bin_width' : 0.005, 'bin_range' : (-3, 3) }
network.hist_args['pathlength'] = { 'bin_width' : 5, 'bin_range' : (0, 50000) }

assert network.rate_matrix(s1.steps, force=True) == network.rate_matrix(s2.steps, force=True)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `926ffdd02aae`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.