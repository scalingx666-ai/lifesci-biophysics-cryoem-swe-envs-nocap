# Issue

**Title:** Minimize replicas sharing context cache

## Description

## Description
This supersedes #818

This set of changes include the changes in #818 but instead of modifying the current minimize replica test in `openmmtools/tests/test_sampling.py::test_minimize`, we implement a new test in `test_cache.py` that checks for context being shared when minimizing replicas with compatible states.

For now we leave the original minimize test as is, even if it has some "dead" code, it enables the possibility of using some integrator as a minimizer, while also not having to change the logic and risking missing corner cases.

## Todos
- [x] Implement feature / fix bug
- [x] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst) to summarize changes in behavior, enhancements, and bugfixes implemented in this PR

## Status
- [x] Ready to go

## Changelog message
```
- Stop removing the barostat before minimizing a replica in ``MultiStateSampler._minimize_replica``.
  ``LocalEnergyMinimizer`` no longer requires a barostat-free system, so minimization now reuses the
  same ``Context`` already cached for that thermodynamic state instead of creating (and leaking, since
  ``energy_context_cache`` is typically unbounded) a separate NVT ``Context`` per replica.
  PR `#XYZ <
- Added regression tests confirming compatible replicas (e.g. differing only by alchemical lambda)
  share a single ``Context`` through minimization, and that the barostat/periodic box are left intact.
```
`XYZ` is just a placeholder for this PR.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `901d8923197a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.