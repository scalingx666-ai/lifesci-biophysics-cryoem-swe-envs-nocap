# Issue

**Title:** Improve ring-break merge detection and expose RDKit MCS kwargs

## Description

This PR fixes a small issue with the ring-break merge detection code where changes in unrelated rings triggered a ring-break warning, i.e. removing one ring and growing another.

The PR also exposes a new `mcs_kwargs` option, allowing the user to tune the options used for the underlying RDKit MCS search. This is useful for certain perturbations, where the default `FESetup`-inspired options are not ideal. I've also added a function to expose the default options such that a user can use these as a base, or compare with them. Previously this was hidden internally, so a user wouldn't actually know what was set.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bb4cc5356631`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.