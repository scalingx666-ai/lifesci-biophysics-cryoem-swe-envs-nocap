# Issue

**Title:** Bug fix

## Description

This pull request refines the handling of thermodynamic integration and simplifies the initialization logic for equilibration control in the `calphy` package. The most important changes include fixing a reference to the `sigma` parameter in the thermodynamic integration method and removing redundant conditional logic for setting the equilibration control.

### Fixes and improvements in thermodynamic integration:

* In `calphy/liquid.py`, corrected the reference to the `sigma` parameter by ensuring it is accessed from `self.calc.uhlenbeck_ford_model` instead of `self.uhlenbeck_ford_model`. This ensures consistency and prevents potential attribute errors.

### Simplification of equilibration control logic:

* In `calphy/phase.py`, removed redundant conditional checks for the reference phase when setting the equilibration control. The logic now directly sets the `equilibration_control` to "nose-hoover" without phase-specific conditions, simplifying the code.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c0632ff1b3a0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.