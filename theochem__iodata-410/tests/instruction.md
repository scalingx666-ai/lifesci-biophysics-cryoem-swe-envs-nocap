# Issue

**Title:** Keep MOL2 atomic charges when a status bit follows them

## Description

In the Tripos MOL2 format, an atom line can carry an optional status bit after the charge, for example:

```
      1 O1          0.0000    0.0000    0.0000 O.3       1 HOH1     -0.8340 BACKBONE
```

`_load_helper_atoms` only read the charge when the line had exactly 9 fields. Lines with a status bit have 10 fields, so their charge was silently replaced by `0.0`:

```python
from iodata import load_one
mol = load_one("water_status_bits.mol2")
print(mol.atcharges["mol2charges"])
# before: [0. 0. 0.]
# after:  [-0.834  0.417  0.417]
```

### Changes
- `iodata/formats/mol2.py`: read the charge whenever there are at least 9 fields.
- Add `iodata/test/data/water_status_bits.mol2` and `test_load_charges_with_status_bits`. The test fails on `main` and passes with the fix.

### Checks
- `pytest -m "not slow"`: 533 passed
- `ruff check` / `ruff format --check`: clean

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9f7e800fc414`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.