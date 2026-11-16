# Issue

**Title:** Take care of coordinate modifcation in selector probability_ratio

## Description

From writing down the code in  I noticed that selector.probability_ratio might not properly work for snapshots that get their coordinates altered.



### How does this fail?
- `selector.probability_ratio` takes a frame, normally the original selected shooting point, the original trajectory, and a new trajectory
-  that frame can be modified before generating the new trajectory, which would lead to the situation that new_traj.index(frame) raises a `ValueError` if the coordinates were modified
- the added test in 3ad2cc3 shows that this behaviour indeed fails

### Why is the ValueError raised when you alter the coordinates, but not with velocity modifications?
- `Trajectory` objects are essentially `list`s and inherits their way of checking for 'index'
- ~~`Trajectory.index(snapshot)` looks at the `hash`es of all internal `Snapshots` and compares them against `hash(snapshot)`~~
(Not true; in general it checks `==`, which for all objects in OPS this checks their `__uuid__` (which is also used as part of the hash))
-  ~~`hash(snapshot)` is determined by the coordinates -> identical coordinates generate identical hashes~~ (This is not true and based on me misremembering things)
- ~~altering the coordinates of a `snapshot` alters the `hash`~~ (This is not true and based on me misremembering things)
-  ~~the hash of original frame does therefor is not found in the altered trajectory, leading to a failure of the `index` call~~

### Why is this only an issue for this selector
-  All other selectors sidestep the issue by not relying on the snapshot hash (either by setting the ratio to 1.0 or the probability of all snapshots to 1.0)

### Solution
This PR adds a keyword argument `new_snapshot` to `probability_ratio`, which defaults to `snapshot` if not given for all internal OPS selectors.

### Possible OPS 2.0 alterations
@dwhswenson  Should we update the call signature of this function to `(old_snapshot, old_trajectory, new_snapshot, new_trajectory)` in 2.0 and raise a Deprecation warning if new_snapshot is not defined for 1.X?

The integration of calling the probability ratio with the new snapshot inside the `EngineMover` will be done as part of #1075

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d28571f7c253`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.