# Issue

**Title:** Feature replica system

## Description

This PR adds a new internal `ReplicaSystem` class to aid rapid setup and tear-down of replica exchange simulations. This works by storing a single system for the shared topology and a trajectory iterator for the coordinates of each replica. With this approach it is possible to use the native parallelism of the Sire trajectory interface to rapidly read and write frames for each replica. The design also allows for large data compression when passing information between stages of a replica exchange workflow. Instead of passing multiple stream files (one for each replica) the user can now pass a single stream file and trajectory.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have added a test for any new functionality in this pull request: [y]
* I confirm that I have added documentation (e.g. a new tutorial page or detailed guide) for any new functionality in this pull request: [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ead409212d64`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.