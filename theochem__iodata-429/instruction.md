# Issue

**Title:** Write an energy of 0.0 and missing orbital energies in the WFN dump

## Description

.

Both points of the issue are fixed in the WFN writer:

- An energy of 0.0 was written as NaN because of `data.energy or np.nan`. Now only `None` gives NaN.
- When the orbital energies are not known (`mo.energies` is `None`, e.g. for natural orbitals), the writer failed with a `TypeError` from the `zip`. Now zeros are written, as the format has an energy for every orbital.

I also looked for the same `x or default` pattern in the other formats and inputs. The only other numeric case is the energy of the WFX writer, which I will do in a separate pull request for #425. The remaining ones are for strings (title, level of theory), where an empty value can safely get the default, or `nelec or 0` and `spinpol or 0` in the FCIDUMP writer, which give the same result.

Two tests are added to `test_wfn.py`. Without the change they fail with `nan == 0.0` and the `TypeError`. The tests of the WFN and WFX modules pass, and `ruff check` and `ruff format` are clean.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `785cf84e7814`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.