# Issue

**Title:** Trajectory Exports: MDTraj and TRR formats; Gromacs and OpenMM support

## Description

Add support in OpenMM and Gromacs engines to export trajectories. This includes lossless export (as TRR) and lossy export (via MDTraj, where the export format will be determined from the filename). We use TRR format as the default lossless format for both OpenMM and Gromacs.

- [x] Lossless: TRRTrajectoryWriter
- [x] Tests for TRRTrajectoryWriter
- [x] Lossy: MDTrajTrajectoryWriter
- [x] Tests for MDTrajTrajectoryWriter
- [x] OpenMMEngine._default_trajectory_writer and tests for OpenMMEngine.export_trajectories
- [x] GromacsEngine._default_trajectory_writer and tests for GromacsEngine.export_trajectories

Overall plan:

* Add support for core functionality and defaults for SimStore exports. (#1170)
* **This PR**: Add support for TRR export for Gromacs and OpenMM engines (as defaults for those engines). Add support for MDTraj-based export for lossy exports.
* Later PR: Add support to export a list of MCSteps, using a given TrajectoryWriter and some definition of where to store step information.
* Later PR: Add new type of storage (based on SimStore) that uses lossless external trajectory storage for snapshot data storage.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `412bdd41f4ca`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.