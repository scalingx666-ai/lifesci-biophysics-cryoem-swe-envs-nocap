# Issue

**Title:** Account for modifier acceptance in engine movers

## Description

for all OPS classes
However, a new issue should be opened to discuss a refactor of `EngineMover` to take this and the other bias options  into account for OPS 2.0.

Custom implementations of both `EngineMover` and `SnapshotModifier` should tyake care to implement this correctly on their end.

Big assumption (that might break API): **A key that starts with "bias" should not be present in `run_details` if it is not intended to be used for the bias calculation of a sample** 

todo:

- [x] add functionality
- [x]  add tests
- [x]  update docstrings
- [x]  make touched files pep8 compliant
- [x]  remove nose from touched tests (partial #756)

commit descriptions:
4f017dd adds probability_ratio to snapshot modifiers
ad7fad0 allows the usage of this probability ratio to be used as a bias in EngineMovers
0e42bb9 reverts an oversight in my thought pattern about the acceptance bias of velocities that are directly sampled from the correct boltzman distribution
ad33488 and b351417 add tests for the added functionality
abf0d99 alters the docstrings that were already present
276e3b3 fixes pep8 complaints from my env
f591ca7 drops nose from the tests
8a6057e Fixes an py2.7 issue in the tests where you can't initiate a class without overwriting *all* abstract methods

[Edit]: added a link to the mentioned comment

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f2c4c1aa0700`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.