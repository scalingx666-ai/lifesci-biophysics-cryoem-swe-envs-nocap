# Issue

**Title:** Reduce the number of calls to NetCDF writing in SamplerState storage

## Description

This changes the `_write_sampler_states_to_given_file` function to put all `SamplerState` positions and box vectors into a single NumPy array before storing to disk. This reduces the number of times the data has to be accessed, which gets slower as the chunk size of the iteration increases (which is now the checkpoint interval). 

This is a pseudo fix to #1008 of which it reduces the write time from 100s to about 5s on 1 GTX 1080. Its still not as fast as if we put the chunksize for the iterations to 1, but its much better. We could reduce the chunksize, but that will then slow down the read access.

Some potential problems which we may have to address later. Not really a "need to answer now," but mostly for documentation.
* We may need to come around on another pass to limit the maximum chunk size so we don't wind up with massive chunks. Not a problem now, but could be with larger checkpoint intervals.
* Systems with huge numbers of particles may run out of memory assigning all replica positions to a single numpy array.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9bd78df1897b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.