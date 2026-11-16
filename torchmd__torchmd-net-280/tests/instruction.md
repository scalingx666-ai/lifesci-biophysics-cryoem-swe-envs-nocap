# Issue

**Title:** Better handling of prior configuration

## Description

I realized there is no way to set prior arguments from the yaml file. Also, the model can handle more than one prior, but there was no way to set more than one from the yaml file.
I updated the documentation to reflect this, which was wrong about it.

With this PR one can do something like:

```yaml
prior_model:
  Atomref:
    max_z: 100
  Coulomb:
      alpha: 1
      max_num_neighbors: 10
      distance_scale: 1
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6d8e3159cfb8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.