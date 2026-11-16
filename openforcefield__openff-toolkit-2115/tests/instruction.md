# Issue

**Title:** Avoid poor scaling of OEChem coordinate lookup

## Description

This change considerably speeds up `Molecule.from_smiles` on a large molecule (not necessarily a key use case, but it happens) from:

```python
In [1]: from openff.toolkit import Molecule

In [2]: %timeit Molecule.from_smiles(1000 * "CCO")
6.57 s ± 120 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```
to:

```python
In [2]: %timeit Molecule.from_smiles(1000 * "CCO")
1.14 s ± 35.4 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```

This change has a 10x speedup when calling `Molecule.from_openeye` on an OEChem representation of a realistic protein:

```python
In [1]: from openff.toolkit import Topology, Molecule

In [2]: t4_oemol = Topology.from_pdb("openff/toolkit/data/proteins/T4-protein.pdb").molecule(0).to_openeye()

In [3]: %timeit Molecule.from_openeye(t4_oemol)  # on commit eea73dca
753 ms ± 9.77 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```

```python
In [3]: %timeit Molecule.from_openeye(t4_oemol)  # this branch, 0f8be0c5504d69a9e7c314879cd3822e6067d13c
66.8 ms ± 629 μs per loop (mean ± std. dev. of 7 runs, 10 loops each)
```

This is almost certainly because of the memory consumption, which balloons without this change. I don't know why - typically memory is freed up after loops, but that's either not happening or it's pointless with the `N_{atoms}` number of calls to `GetCoords`. I re-ran Lily's reproduction in #1855 and the total memory usage drops from just under 2 GB:

<img width="1822" height="1180" alt="image" src="https://github.com/user-attachments/assets/0f08bda6-34ed-4da2-aba9-2370a2f02ffb" />

to around a tenth of that, almost exclusively upstream imports:

<img width="1822" height="1180" alt="image" src="https://github.com/user-attachments/assets/669b76cc-2170-4526-879a-ae50e8d9900d" />

I believe this is the underlying issue of #1855 so I'm linking this PR to it
- [ ] Tag issue being addressed
- [ ] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [ ] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [ ] [Lint](https://docs.openforcefield.org/projects/toolkit/en/stable/users/developing.html#style-guide) codebase
- [ ] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `69fe926ad38f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.