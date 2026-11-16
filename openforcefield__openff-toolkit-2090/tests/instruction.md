# Issue

**Title:** Improve speed of quantity stringification

## Description

Relates to #2065, split off from #2081

I see a ~20% speedup in stringifying some force fields

```python
In [1]: import openff.toolkit

In [2]: ff14sb = openff.toolkit.ForceField("ff14sb_off_impropers_0.0.4.offxml")
/Users/mattthompson/micromamba/envs/openff-toolkit-test/lib/python3.12/site-packages/openff/amber_ff_ports/amber_ff_ports.py:8: UserWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html. The pkg_resources package is slated for removal as early as 2025-11-30. Refrain from using this package or pin to Setuptools<81.
  from pkg_resources import resource_filename
/Users/mattthompson/micromamba/envs/openff-toolkit-test/lib/python3.12/site-packages/openff/amber_ff_ports/amber_ff_ports.py:8: UserWarning: Module openff was already imported from None, but /Users/mattthompson/software/openff-toolkit is being added to sys.path
  from pkg_resources import resource_filename

In [3]: %timeit ff14sb._to_smirnoff_data()
388 ms ± 2.91 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```
This is compared to 
```python
501 ms ± 24.4 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```
upstream, on my machine

- [ ] Tag issue being addressed
- [ ] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [ ] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [ ] [Lint](https://docs.openforcefield.org/projects/toolkit/en/stable/users/developing.html#style-guide) codebase
- [ ] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `158117d535ba`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.