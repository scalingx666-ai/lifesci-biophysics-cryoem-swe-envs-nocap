# Issue

**Title:** Add global random number generator; support in shooting pt selectors and pathmovers

## Description

This week my CI failed with:
```python
=================================== FAILURES ===================================
______________________ TestGaussianBiasSelector.test_pick ______________________

self = <openpathsampling.tests.test_shooting.TestGaussianBiasSelector object at 0x7fac57d4fc10>

    def test_pick(self):
        picks = [self.sel.pick(self.mytraj) for _ in range(100)]
        pick_counter = collections.Counter(picks)
        assert set(pick_counter.keys()) == set(range(len(self.mytraj)))
        # final test: 99.5 should happen more than 32.4
>       assert pick_counter[2] > pick_counter[0]
E       assert 13 > 18

openpathsampling/tests/test_shooting.py:70: AssertionError
```

This PR attaches a random number generator to the selectors that can be seeded during tests to prevent these flaky tests.

The current implementation requires `numpy>=1.17` (that is when numpy swapped from `RandomState` to [`Generator`](https://numpy.org/doc/stable/reference/random/index.html)) @dwhswenson please let me know if that is too strict of a requirement and if I should write some code to support the Legacy `RandomState`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5f8603273871`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.