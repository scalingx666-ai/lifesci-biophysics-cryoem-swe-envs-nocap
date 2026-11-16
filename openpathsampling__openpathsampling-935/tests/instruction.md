# Issue

**Title:** Improve CV storage error message about missing templates

## Description

In netcdfplus, storing CVs (`PseudoAttribute`s) requires a template object to be already stored. Originally we only had CVs that mapped snapshots to computed values, but eventually @jhprinz added the ability to use other objects (like trajectories) as inputs. However, the error message didn't get updated, and it is very confusing to get an error message saying to save a snapshot when you already have a snapshot saved (but need to save a trajectory).

This whole issue will be irrelevant with SimStore, but might as well fix the confusing error message.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5570cd317230`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.