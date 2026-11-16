# Issue

**Title:** Fix Cerberus and support for trailblaze options

## Description

* Fix #1141: YANK is now compatible with Cerberus >= 1.2, which means we should be able to remove cerberus from omnia.
* Fix #1139: Expose more options for the trailblaze algorithm in YAML. It is now possible to tweak the trailblaze runs. This is the example that I added to the Sphinx docs.

```yaml
protocols:
  {UserDefinedProtocol}:
    {PhaseName1}:
      trailblazer_options:
        # Number of initial equilibration iterations performed in the first state before running the algorithm.
        n_equilibration_iterations: 1000
        # Whether the heavy atoms of the receptor are positionally restrained with a harmonic potential during the algorithm.
        constrain_receptor: false
        # Number of samples used to estimate the standard deviation of the potential between two states.
        n_samples_per_state: 100
        # The "distance" in potential standard deviation between two intermediate states up to 'threshold_tolerance'.
        std_potential_threshold: 0.5  # in kT
        threshold_tolerance: 0.05  # in kT
        # Whether to traverse the path in the forward (given by 'alchemical_path') or reversed direction.
        reversed_direction: true
        # A variable controlling the path if 'alchemical_path' contains mathematical expressions.
        function_variable_name: lambda

      alchemical_path:
        # Deactivate electrostatic interactions before decoupling the Lennard-Jones potential.
        lambda_electrostatics: 2*(lambda - 0.5) * step(lambda - 0.5)
        lambda_sterics: step_hm(lambda - 0.5) + 2*lambda * step_hm(0.5 - lambda)
        lambda: [1.0, 0.0]
```
``alchemical_path: auto`` is still possible, with or without the new block ``trailblazer_options``.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `61cf64c184ae`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.