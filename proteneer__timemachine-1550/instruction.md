# Issue

**Title:** Improved nonbonded interpolation schedule.

## Description

This PR improves the nonbonded interpolation schedule for replacement types of transformations. Some key changes are:

1) For insertions, the dummy group starts off with a scaled down epsilon and is fully decharged. It's `w`-coordinate is scaled from `cutoff` to `0` between `lambda=0` and `lambda=1/3`. From `lambda=1/3` to `lambda=2/3`, the epsilons and charges are linearly scaled up to the fully interacting values. 
2) At `lambda=0.5`, both R-groups are interacting with the environment.
3) The net charge is preserved throughout the lambda schedule. i.e:
``` python
np.testing.assert_array_almost_equal(np.sum(guest_charges), np.sum(guest_a_q), decimal=3)
np.testing.assert_array_almost_equal(np.sum(guest_charges), np.sum(guest_b_q), decimal=3)
```
4) New plotting code is added, splitting nonbonded and bonded interpolation schemes into two separate diagrams.
5) For a couple of identity transformations, roughly `10-20%` fewer lambda windows are observed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `831a6572bc53`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.