# Issue

**Title:** Perf/verify_chiral_consistency_of_core

## Description

Implements the second option in  , maybe ~10x faster on an example than using the `"smirnoff_1_1_0_sc.py"` file.

<details>

```python
from timemachine.fe.single_topology import verify_chiral_consistency_of_core
from timemachine.testsystems.relative import get_hif2a_ligand_pair_single_topology
from timemachine.ff import Forcefield

mol_a, mol_b, core = get_hif2a_ligand_pair_single_topology()

default_ff = Forcefield.load_default()
sc_ff = Forcefield.load_from_file("smirnoff_1_1_0_sc.py")
placeholder_ff = Forcefield.load_from_file("placeholder_ff.py")

# ~500ms
_ = verify_chiral_consistency_of_core(mol_a, mol_b, core, default_ff)

# ~500ms
_ = verify_chiral_consistency_of_core(mol_a, mol_b, core, sc_ff)

# ~30ms
_ = verify_chiral_consistency_of_core(mol_a, mol_b, core, placeholder_ff)
```

</details>

Adds a `"placeholder_ff.py"` file (with a single generic type per potential) to `ff/params` in case useful in similar contexts. (wherever it might be convenient to go through a `.parameterize(mol)` code path to get `potential.idxs` but `potential.params` would be unnecessary, and you want to minimize the number of unnecessary smarts queries.)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b7d555037d32`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.