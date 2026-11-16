# Issue

**Title:** support interpolation in sel"" macro.

## Description

With this modification, the `sel""` raw-string macro supports interpolated variables. That is, previously, this didn't work:

```julia
atom_name = "CA"
collectatoms(struc, sel"name $atom_name")
```

now it does.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a2e3fa0ad0d5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.