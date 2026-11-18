# Issue

**Title:** User exposing init and step for customization.

## Description

# User exposing init and step

Currently, the only user API to solve a cell ODE is `jx.integrate`. For various applications, it is useful to have access to an `init` function, which initializes the state, and a `step` function, which executes a single ode step. Some examples are:
- Nonuniform step sizes
- Integration with Kalman or Particle filtering frameworks
- Stochastic Models (i.e. add noise after each step) ...

Also  .

## Proposal


```python
init_fn, step_fn = build_init_and_step_fn(cell, **maybe_some_other)

# All the reformating
states, params = init_fn(params, all_states, **kwargs)
next_state = step_fn(states, params, externals, dt=0.025, **kwargs)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `61c5c276c867`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.