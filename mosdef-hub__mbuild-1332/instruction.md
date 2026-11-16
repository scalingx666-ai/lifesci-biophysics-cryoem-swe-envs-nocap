# Issue

**Title:** Fix key for opls in forces dict, use rtol in tests comparing energies

## Description

### PR Summary:

The key for `opls` in the forces dict was throwing a KeyError when `scale_opls` was a non-zero value. We didn't have a test for this, its added (error shown below, but now fixed with the changes). I also added `rtol` for the tests that are comparing energies against reference values.

```
(mbuild-dev) ❯  mbuild git:(develop) ● pytest tests/test_simulation.py -k opls
====================================================================== test session starts ======================================================================
platform linux -- Python 3.13.0, pytest-9.0.3, pluggy-1.6.0
rootdir: /home/chris/dev/forks/mbuild
configfile: pyproject.toml
plugins: requests-mock-1.12.1, cov-7.1.0, anyio-4.13.0
collected 25 items / 24 deselected / 1 selected

tests/test_simulation.py F                                                                                                                                [100%]

=========================================================================== FAILURES ============================================================================
______________________________________________________________ TestSimulationHoomd.test_scale_opls ______________________________________________________________

self = <mbuild.tests.test_simulation.TestSimulationHoomd object at 0x7fa4771c88a0>, sim = <mbuild.simulation.HoomdSimulation object at 0x7fa4771517f0>

    def test_scale_opls(self, sim):
        ffhandler = ForcesHandler(scale_opls=0.5)
>       ffhandler.scale_sim(sim)

/home/chris/dev/forks/mbuild/mbuild/tests/test_simulation.py:406:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
/home/chris/dev/forks/mbuild/mbuild/simulation.py:364: in scale_sim
    force.params[param][term] *= scalar
    ^^^^^^^^^^^^^^^^^^^^^^^^^
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

self = _HOOMDDict{'k1': 0.0, 'k2': -0.0, 'k3': 1.2552, 'k4': -0.0}, key = ('k1', 'k2', 'k3', 'k4')

    def __getitem__(self, key):
        self._read()
>       return self._data[key]
               ^^^^^^^^^^^^^^^
E       KeyError: ('k1', 'k2', 'k3', 'k4')

/home/chris/miniforge3/envs/mbuild-dev/lib/python3.13/site-packages/hoomd/data/collections.py:332: KeyError

```

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3843db014e75`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.