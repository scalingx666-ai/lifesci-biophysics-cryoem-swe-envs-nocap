# Issue

**Title:** Improve SAMS State Histograms and Refactor from_storage

## Description

Given the large number of changes, and some things which need reviewed, I made this
into a PR instead of quick fix.

This changes how SAMS gets it `state_histograms` by storing a shape `[n_states]`
vector each iteration to the same location instead of relying on the
`reporter.replica_thermodynmaic_states` variable of shape `[iteration, n_states]`
which gets slower to access as `iteration` increases due to NetCDF file chunking.
Implemented on request of @jchodera

I would also like some feedback regarding how I [changed the online-free-energy test](https://github.com/choderalab/yank/compare/sams...histograms_and_storage?expand=1#diff-dd3d51a3d6465f978ab53b80e79929c1R1508).
Since SAMS may not sample well enough in the short run for MBAR to converge, I added
a test extension which tries to run longer up to a hard limit to see if more samples
fix the undersampling problem. We'll eventually refine that test to better
handle SAMS in the future, but I think this should work for now.

The MultistateSampler `from_storage` function has been refactored and split into
3 functions:
* Public `from_storage` function with an unchanged signature, calls the other 2
* `_instantiate_sampler_from_storage` class method which simply calls the `__init__` of the sampler after reading options from storage object
* `_restore_sampler_from_reporter` instance method which restores the state of the sampler from a reporter object.

This break down allows subclasses to implement either of the hidden methods to help with
restoring the state in a more granular way instead of having to rely on making a
full `from_storage` sub-method which does everything for the parent, but then the child
would need to re-open the reporter, reapply the settings, and it annoyed the IDE's
since they thought the `super().from_storage` returned an instance of the parent, and
not the child, making it complain about any additional calls in the `from_storage`
sub method unique to the child
Could use review @andrrizzi if possible (code changes shorter than reasoning)

Also fixes a bug in the online-analysis storage subroutines of the `Reporter` where
searching for alternate variable names threw a TypeError due to a convenience thing netCDF4's tries to do with key processing not handling the `in` method the way I figured it would.

Finally, I made the sampler base test class in `test_sampling.py` print out something
more helpful when it starts in on a new sampler when `--nocapture` is set. Should
help us debug a bit faster in the future.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6700b44256dc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.