# Issue

**Title:** MMTBX: remove comparison of int with NoneType

## Description

Fix a couple of places in `mmtbx.scaling` where the attribute of a class, initially set to `None`, is implicitly expected to have since become an `int`.  There are seemingly valid paths through the code in which the attribute values remain `None`, so this leads to a comparison `TypeError`.  Removing these code paths seems pretty trivial.

@phyy-nx and I stubbed our toes on this last week when trying to run [`xia2.multiplex`](https://dials.github.io/documentation/programs/xia2_multiplex.html), of which MMTBX is a dependency.

I've no idea why tests are failing in the CI.  They don't, at first glance, seem to be related to these changes.  @bkpoon, could you please advise as to how you would like me to proceed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b89a36e7583c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.