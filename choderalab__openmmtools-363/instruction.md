# Issue

**Title:** GlobalParameterState and changes for multi-restraint calculations

## Description

- Deprecated the signature of `IComposableState._on_setattr` to fix a bug where the objects were temporarily left in an inconsistent state when an exception was raised and caught.
- Deprecated `update_alchemical_charges` in `AlchemicalState` in anticipation of the new implementation of the exact PME that will be based on the `NonbondedForce` offsets rather than `updateParametersInContext()`.
- Allow restraint force classes to be controlled by a parameter other than `'lambda_restraints'`. This will enable multi-restraints simulations.
- Implemented `states.GlobalParametersState`. This more or less generalize the logic of `AlchemicalState` and implement the "suffix" mechanism that we'll be able to use in multi-restraint calculations to control parameters such as `lambda_restraints_restraint1`, `lambda_restraints_restraint2` and, in the future, in multiple alchemical region calculations. With this, the new `RestraintState` class in YANK can be simplified to:
 ```python
class RestraintState(GlobalParameterState):
    lambda_restraints = GlobalParameterState.GlobalParameter('lambda_restraints', standard_value=1.0)

    @lambda_restraints.validator
    def lambda_restraints(self, instance, new_value):
        if not (0.0 <= new_value <= 1.0):
            raise ValueError('lambda_restraints must be between 0.0 and 1.0')
        return new_value

    # A constructor just to give parameters_name_suffix a more meaningful name.
    def __init__(restraint_name=None, **kwargs):
        super().__init__(parameters_name_suffix=restraint_name, **kwargs)
```
`AlchemicalState` will be similarly simplified, but we'll have to wait to do it until the new exact PME implementation will be in.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8f42ba26f56c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.