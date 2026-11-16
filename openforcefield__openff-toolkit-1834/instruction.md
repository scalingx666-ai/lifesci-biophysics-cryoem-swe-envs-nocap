# Issue

**Title:** Add supported charge methods to API

## Description

Implements #1830

```python
In [1]: from openff.toolkit.utils import *

In [2]: for _class in GLOBAL_TOOLKIT_REGISTRY.registered_toolkits:
   ...:     print(_class.__class__.__name__, _class.supported_charge_methods)
   ...:
OpenEyeToolkitWrapper ['am1bcc', 'am1-mulliken', 'gasteiger', 'mmff94', 'am1bccnosymspt', 'am1elf10', 'am1bccelf10']
RDKitToolkitWrapper ['mmff94', 'gasteiger']
AmberToolsToolkitWrapper ['am1bcc', 'am1-mulliken', 'gasteiger']
BuiltInToolkitWrapper ['zeros', 'formal_charge']

In [3]: from openff.toolkit import Molecule

In [4]: molecule = Molecule.from_smiles("CCO")

In [5]: molecule.get_available_charge_methods()
Out[5]:
['am1elf10',
 'am1bccelf10',
 'zeros',
 'am1bcc',
 'gasteiger',
 'am1-mulliken',
 'am1bccnosymspt',
 'formal_charge',
 'mmff94']

In [6]: from openff.toolkit.utils.nagl_wrapper import NAGLToolkitWrapper

In [7]: NAGLToolkitWrapper().supported_charge_methods
Out[7]: ['openff-gnn-am1bcc-0.0.1-alpha.1.pt', 'openff-gnn-am1bcc-0.1.0-rc.1.pt']

```

- [x] Tag issue being addressed
- [x] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [ ] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://open-forcefield-toolkit.readthedocs.io/en/latest/developing.html#style-guide) codebase
- [ ] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.rst)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `858d55e73332`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.