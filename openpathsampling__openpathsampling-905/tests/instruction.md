# Issue

**Title:** DirectSimulation should use engine.start/.stop

## Description

From an email report from a user:

> I tried to follow this example: 
>
> http://openpathsampling.org/latest/examples/mistis.html#toy-model-mistis
>
> to set up openpathsampling.DirectSimulation for my flux_pairs, but got this error: 
> 
> ```python
> AttributeError: 'GromacsEngine' has no attribute 'n_frames_since_start', nor does its options dictionary
> ```

Two problems, both solved in this PR:

1. `ExternalEngine.n_frames_since_start` is only set in its `.start()` method. It should be set (to `None`) in `__init__`.
2. `DirectSimulation` is not using the `.start()` and `.stop()` methods. This didn't matter for OpenMM/toy engine, but does for external engines like Gromacs.

NOTE: I would not recommend using `DirectSimulation` on external engines. `DirectSimulation` is mainly useful to get fluxes from toy engine runs or other simulations where you don't want to save trajectory frames. It's better to run a trajectory normally and use `TrajectoryTransitionAnalysis`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `26997b43745e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.