# Issue

**Title:** Shooting point selector with arbitrary bias

## Description

For a very long time, OPS has had the ability to use a Gaussian bias in shooting point selection. That is, given a CV `f`, the  probability of selecting a shooting point `x` within a given trajectory is proportional to `np.exp(-a*(f(x)-x0)**2)`, where `a` and `x0` are parameters set by the user.

There is no reason for this to be restricted to Gaussians. This PR introduces a `BiasedSelector`, which allows one to use *any* function (wrapped in an OPS CV) as the bias function. The only reason this didn't exist before is because when we were writing shooting point selectors, I don't think we understood the flexibility that OPS CVs would have. 

Now, for example, the `GaussianBiasSelector` could be implemented as:

```python
def gaussian(snapshot, cv, a, x0):
    return np.exp(-a * (cv(snapshot)-x0)**2)

cv_gaussian = paths.FunctionCV('shooting_bias', gaussian, cv=my_cv, a=2.0, x0=0.25)
selector = paths.BiasedSelector(cv_gaussian)
```

But similarly, one could use any other function.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5e1d2f6b8511`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.