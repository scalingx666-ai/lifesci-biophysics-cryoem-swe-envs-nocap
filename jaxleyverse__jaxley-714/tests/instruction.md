# Issue

**Title:** safe_softplus

## Description

Our implementation of `softplus` in parameter transforms makes it return 20.0 if the parameter is larger than 20.0, as we implement it as `jnp.log1p(save_exp(x))` and `save_exp` clips values above 20.0. 

A safe way to counter this is to just use the linear function if the input is bigger than a certain threshold (e.g., 20.0 like the save_exp). This is also what is done in PyTorch for example.

For example:

```
def softplus_save(x, lower=0.0):
    return jnp.where(x > 20.0, x, jnp.log1p(save_exp(x))) + lower

def softplus(x, lower=0.0):
    return jnp.log1p(save_exp(x)) + lower

print(softplus_save(25))
print(softplus(25))
```
results in
```
25.0
20.0
```
25.0 is desirable I'd think!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a368f5be6efa`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.