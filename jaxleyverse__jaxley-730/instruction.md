# Issue

**Title:** feat: Utilities to extract the dynamic (ODE) states of a module

## Description

Code in this PR was by and large written by @Matthijspals.

This PR adds utilities to process the ``all_states`` dictionary such that it only contains "true" ODE states:
- it removes branchpoint states
- it removes membrane states that are NaN because the channel does not exist there
- it removes ionic currents.

In addition, the utilities can flatten the states to a vector.

These utilities make it easier to build, for example, the jacobian, and they also make interoperability with frameworks like [dynamax](https://probml.github.io/dynamax/) easier.

At its core, this functionality is executed by four utilities:
```python
from jaxley.utils.dynamics import build_dynamic_state_utils
remove_observables, add_observables, flatten, unflatten = build_dynamic_state_utils(cell)
```

Simplest usage is:
```python
import jaxley as jx
from jaxley.integrate import build_init_and_step_fn
from jaxley.utils.dynamics import build_dynamic_state_utils

cell = jx.Cell()
params = cell.get_parameters()

init_fn, step_fn = build_init_and_step_fn(cell)
remove_observables, add_observables, flatten, unflatten = build_dynamic_state_utils(cell)

all_states, all_params = init_fn(params)

dynamic_states = flatten(remove_observables(all_states))
recovered_all_states = add_observables(unflatten(dynamic_states), all_params, delta_t=0.025)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9f7579acc817`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.