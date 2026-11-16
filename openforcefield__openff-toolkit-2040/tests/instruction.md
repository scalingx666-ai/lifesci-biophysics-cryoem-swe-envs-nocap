# Issue

**Title:** Support large molecule InChi with RDKit

## Description

Partial fix for #1977 since OEChem insists on not allowing the user to pass the argument through to InChI's tool

Arguably the `/LargeMolecules` option should be opt-in, but that's an API addition with no obvious upside.

This would change, but not break, how NAGL uses InChI to key its lookup table (). Currently, it makes an attempt to gracefully handle `EmptyInChiError`; when RDKit is that case shouldn't be needed any more.

This still appears to be relatively fast for a large molecule, not accounting for the time it takes to instantiate `RDKitToolkitWrapper`, delegate out, etc.:

```python
In [6]: molecule = Molecule.from_smiles(1000 * "C")

In [7]: %timeit molecule.to_inchi(toolkit_registry=RDKitToolkitWrapper())
39.7 ms ± 1.54 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```

Simple demo:

```python
from openff.toolkit import Molecule
from openff.toolkit.utils.rdkit_wrapper import RDKitToolkitWrapper, EmptyInChiError

try:
    Molecule.from_smiles(342 * "C").to_inchi(toolkit_registry=RDKitToolkitWrapper())
except EmptyInChiError as error:
    print(error)
else:
    print("InChI fine ... ")

try:
    Molecule.from_smiles(342 * "C").to_inchikey(toolkit_registry=RDKitToolkitWrapper())
except EmptyInChiError as error:
    print(error)
else:
    print("InChI key fine ... ")
```
Out:
```console
$ git checkout upstream/main && python large_molecule_inchi.py
HEAD is now at 4cc57064 re-ignore external examples in conda tests (#2038)
[13:51:05] ERROR: Too many atoms [did you forget 'LargeMolecules' switch?]

RDKit failed to generate an InChI for the molecule.
[13:51:05] Invalid InChI prefix in generating InChI Key
RDKit failed to generate an InChI key for the molecule.
(openff-toolkit-test) [openff-toolkit]
$ git checkout rdkit-large-molecule-inchi && python large_molecule_inchi.py
Previous HEAD position was 4cc57064 re-ignore external examples in conda tests (#2038)
Switched to branch 'rdkit-large-molecule-inchi'
InChI fine ...
InChI key fine ...
```

- [x] Tag issue being addressed
- [x] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [x] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://docs.openforcefield.org/projects/toolkit/en/stable/users/developing.html#style-guide) codebase
- [x] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4cc5706446ff`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.