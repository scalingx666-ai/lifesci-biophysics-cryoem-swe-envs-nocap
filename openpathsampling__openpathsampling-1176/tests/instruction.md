# Issue

**Title:** Trajectory Export: Multiple Steps

## Description

This exports trajectories from a set of MC steps, using symlinks to prevent duplication. There will be snapshot duplication when two trajectories share snapshots (one-way-shooting) and coordinate duplication when two trajectories are reversed (path reversal), but full trajectories, which may not change across MC steps (e.g., shooting in only one ensemble leaves the others unchanged), are not duplicated.

The default file structure is, e.g.:

```
100/
├── active
│   ├── condensed MIS minus.pdb -> ../../raw_data/17290287612105036191526417646194015568.pdb
│   ├── condensed->extended 0.pdb -> ../../raw_data/17290287612105036191526417646194017830.pdb
│   ├── condensed->extended 1.pdb -> ../../raw_data/17290287612105036191526417646194017878.pdb
│   └── condensed->extended 2.pdb -> ../../raw_data/17290287612105036191526417646194017276.pdb
└── trials
    └── condensed->extended 1.pdb -> ../../raw_data/17290287612105036191526417646194018084.pdb
raw_data/
├── 17290287612105036191526417646194015568.pdb
├── 17290287612105036191526417646194017830.pdb
├── 17290287612105036191526417646194017878.pdb
├── 17290287612105036191526417646194017276.pdb
├── 17290287612105036191526417646194018084.pdb
```

where, in that example, 100 is the step number. The files in `active` and `trials` are named by the friendly name of the ensemble, and are symlinks to data in `raw_data`, named by the UUID.

I've also changed it so that tracking the extension is the job of the writer. This does mean that the writer is more complicated than just a bare function, but it feels to me like it makes more sense.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `10a5729b3d15`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.