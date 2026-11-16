# Issue

**Title:** fix bug in MC anisotropic barostat

## Description

The length scaling factor in the anisotropic barostat is currently:

```julia
l_scale = SVector{D}(mask1 * (D == 2 ? sqrt(v_scale) : cbrt(v_scale)) + mask2)
```

However, since in the anisotropic case only one axis is scaled at a time, it should be replaced to:

```julia
l_scale = SVector{D}(mask1 * v_scale + mask2)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0e80b6c7d63d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.