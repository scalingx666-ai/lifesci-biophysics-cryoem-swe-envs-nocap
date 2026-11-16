# Issue

**Title:** WIP: remove runtime error in pytraj.topology.topology.Topology.to_parmed.

## Description

When the to_parmed method of the pytraj.topology.topology.Topology class is invoked, the
I got the following error.
```
  File "pytraj/topology/topology.pyx", line 874, in pytraj.topology.topology.Topology.to_parmed
  File "pytraj/topology/topology.pyx", line 875, in pytraj.topology.topology.Topology.to_parmed
  File "pytraj/topology/topology.pyx", line 837, in pytraj.topology.topology.Topology.save
TypeError: save() got an unexpected keyword argument 'overwrite'
```
python: 3.8.1
cpptraj: V4.26.2 (GitHub)
pytraj: 2.0.5.dev0
ParmEd: 3.2.0

The reason for this error is that Topology.save does not have an argument 'overwrite'.
So, I added 'overwrite' to the argument of ParmFile.write as well as Topology.save.

Of course, we can also remove overwrite=True from Topology.to_parmed (line 875 of topology.pyx).
Which solution would be more preferable?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1e8f472e3e18`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.