# Issue

**Title:** Add anisotropic MC barostat

## Description

This PR adds an implementation for the anisotropic Monte Carlo barostat following OpenMM's implementation.

The new interface for using this barostat in 3D and 2D systems is:

```julia
MonteCarloAnisotropicBarostat(
    1.0u"bar", # Pressure in the X-axis
    1.0u"bar", # Pressure in the Y-axis
    1.0u"bar", # Pressure in the Z-axis
    temperature,
    boundary
)
```
and
```julia
MonteCarloAnisotropicBarostat(
    1.0u"bar*nm", # Pressure in the X-axis
    1.0u"bar*nm", # Pressure in the Y-axis
    temperature,
    boundary
)

```
respectively.

If you set any pressure value as 'nothing', the barostat is uncoupled in that axis. For example, for coupling only the Y-axis, you could do the following:

```julia
MonteCarloAnisotropicBarostat(
    nothing, # Keep the box in the X-axis fixed
    1.0u"bar", # Scale box in the Y-axis
    nothing, # Keep the box in the Z-axis fixed
    temperature,
    boundary
)
```

After discussing and merging this PR, I will start working on the constant surface tension MC barostat for membrane simulations.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `38dec32b1668`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.