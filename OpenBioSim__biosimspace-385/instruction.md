# Issue

**Title:** Fix issues 381, 382, and add a Mass type

## Description

This PR  by adding a `._to_default_unit()` method to the internal `GeneralUnit` class. This simply returns `self`, since they are by definition in internal default units.

The PR also  by clarifying the use of the `explicit_dummies` kwarg in the `Process.Amber` constructor docstring.

The PR also adds support for a new `Mass` type, a `Mass` requirement for nodes, and `Mass` unit convenience classes.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have added a test for any new functionality in this pull request: [y]
* I confirm that I have added documentation (e.g. a new tutorial page or detailed guide) for any new functionality in this pull request: [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `92662d6e3291`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.