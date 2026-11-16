# Issue

**Title:** Implement SAMS sampler

## Description

This supercedes #886.

@Lnaden : I need your help adding writing/reading logZ to the storage layer in an appropriately modular way. I'd like to store it without having to implement a ton of extra code to subclass things. We have a couple of options:

Option 1: Reuse `log_f_k`. It could make sense to co-opt the online analysis framework for SAMS, since it provides its own online analysis, but it would also require overriding or restructuring the way we do online analysis right now, which is very MBAR-centric. I'd like to overhaul that, but maybe later. Note that `logZ[k] = - log_f_k[k]` in my convention, so we can easily store one or the other---no real need to store both.

Option 2: Make it easier to extend what kinds of data are stored to / resumed from the storage. Is there some way we can make it really easy for subclasses to simply specify they want to store more data without having to subclass a ton of things? Right now, it seems like this would require
* Creating a new `SAMSReporter` that has its own copy of `__init__`
* Implementing `write_X` and `read_X` in `SAMSReporter`
* Implementing `from_storage` and `_report_iteration` to first call `MultiStateSampler` versions of these methods and then `read_X` or `write_X`

There may still be other bugs to squish, but resuming/storing `logZ` is the biggest challenge so far.

Once we get this implemented, we still need a way to test, since there is no way to select the sampler via the YAML file yet.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `445dc4212871`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.