# Issue

**Title:** Trajectory Exports: Core and integration in `DynamicsEngine`

## Description

One of the major things that came out of the path sampling software meeting in Leiden in September 2024 was a desire for OPS to export trajectories more easily into formats that people are more familiar with.

In this PR, I'm starting to add functionality for external trajectory export. The eventual goal is that you'll be able to export a trajectory using `engine.export_trajectory(trajectory, filename)`. That method will take an optional `writer` parameter that takes a callable to perform the actual output. This allows a single unified interface from the user perspective, but allows individual callable (as, e.g., instances of a class) to handle details of implementation; for example, allowing lossy output while defaulting to lossless. Different engines can also have different default trajectory writers.

This PR will add support for this in the base `DynamicsEngine` class, where the `_default_trajectory_writer` will return a `SimStoreTrajectoryWriter`, since all engines should be able to export in that format. A future PR will add better support for OpenMM and Gromacs engines, as well as lossy output using MDTraj.

- [x] `SimStoreTrajectoryWriter`
- [x] Tests for `SimStoreTrajectoryWriter`
- [x] `DynamicsEngine.export_trajectory`
- [x] Tests for `DynamicsEngine.export_trajectory`
- [x] Add to developer docs to explain this to engine developers.

Overall plan:
1. **This PR**: Add support for core functionality and defaults for SimStore exports.
2. Upcoming PR: Add support for TRR export for Gromacs and OpenMM engines (as defaults for those engines). Add support for MDTraj-based export for lossy exports.
3. Later PR: Add support to export a list of `MCStep`s, using a given `TrajectoryWriter` and some definition of where to store step information.
4. Later PR: Add new type of storage (based on SimStore) that uses lossless external trajectory storage for snapshot data storage.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cb981d836a5b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.