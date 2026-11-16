# Issue

**Title:** remove deprectated np.float alias

## Description

## Description
With numpy `1.20` released, a downstream project (`openpathsampling`) showed deprecation warnings coming from this code. This fixes that.

## Todos
- [X] Implement feature / fix bug
- [NA] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [NA] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [X] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)

## Status
- [X] Ready to go


## Warning
```
DeprecationWarning: `np.float` is a deprecated alias for the builtin `float`. To silence this warning, use `float` by itself. Doing this will not modify any behavior and is safe. If you specifically wanted the numpy scalar type, use `np.float64` here.
  Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations
    self._positions = unit.Quantity(np.zeros([0, 3], np.float), unit.nanometers)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f78d738fe936`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.