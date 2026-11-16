# Issue

**Title:** implement MC membrane barostat

## Description

This PR adds an implementation for the Monte Carlo membrane barostat following OpenMM's implementation.

This barostat is only available for 3D systems, and the API is as follows:

```julia
MonteCarloMembraneBarostat(
    1.0u"bar",   # Pressure in the system
    1.0u"bar*nm" # Surface tension
    temperature,
    boundary;
    xy_isotropy=false,        # If true, scale the XY-axes isotropically
    constant_volume=false,    # If true, keep the volume constant by scaling the Z-axis accordingly
    z_axis_fixed=false        # If true, keep the Z-axis fixed
)
and
```

In this barostat, `constant_volume` and `z_axis_fixed` cannot be true simultaneously.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d7a0d9032e7e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.