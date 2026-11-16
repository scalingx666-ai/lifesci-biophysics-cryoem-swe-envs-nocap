# Issue

**Title:** Prevent excessive trajectory slicing

## Description

Some more UUID optimization coming from #892.

So while running the test uuid system from #892, I noticed **a lot** of `Trajectory` objects getting generated. Looking at the code, **every time you slice a Trajectory, you generate a new Trajectory**.

To test how many and were I added the following code to trajectory.py (and where) 
```diff
+++ b/openpathsampling/engines/trajectory.py
@@ -23,6 +23,7 @@ class Trajectory(list, StorableObject):
     """
 
     engine = None
+    kill_count = 1000000
 
     def __init__(self, trajectory=None):
         """
@@ -45,6 +46,10 @@ class Trajectory(list, StorableObject):
             else:
                 self.extend(trajectory)
 
+        Trajectory.kill_count -= 1
+        if Trajectory.kill_count < 0:
+            raise RuntimeError("KILLED")
+
```

and ran the following script:
```python
sset = testscheme.initial_conditions_from_trajectories([eng_trj, eng_trj2,
                                                        eng_trj3],
                                                        strategies=[
                                                            'split',
                                                            'extend-complex',
                                                            'extend-minimal'])
# Error out early
sset.sanity_check()
testscheme.assert_initial_conditions(sset)

#Equilibrate
equilibration = paths.PathSampling(
    storage=None,
    sample_set=sset,
    move_scheme=testscheme
)
initial = 100000
paths.Trajectory.kill_count=initial
equilibration.run(10)
print("trajs_generated = "+str(initial-paths.Trajectory.kill_count))
```
(important part here is that I only look at the number of trajectories created after initialization)

Here is a table of number of trajectories created from running this 6 times on `master` and 6 times on this branch
|  | main| this PR |
| --- | --- | --- | 
||77857|20728|
||64553|22852|
||73042|14508|
||76004|18205|
||75349|27016|
||82440|33964|
|*mean*| 74874 | 22879 |

This PR saves **70%** of all Trajectory objects generated (during sampling). `23_000` still an insane amount in my opinion for just `10` mc steps, but this is at least a decent step into (hopefully) not running into #892 as often.

In the long term; sliced trajectories should probably return read_only views (without an uuid)  and not new Trajectories

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `82d5948dfe6f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.