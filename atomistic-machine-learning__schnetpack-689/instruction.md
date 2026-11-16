# Issue

**Title:** [Bug] `GroupSplit` missing in QM7X dataset

## Description

# General

Hi all. Thanks for the work on this project. I noticed two issues while working with the QM7X dataset:
- Missing symbol export from splitters.py (missing `GroupSplit` used by the `QM7X` dataset)
- Missing requirement for `progressbar` (only used in `QM7X`)

# Details

## Minimal Working Example

```python
from schnetpack.datasets import qm7x

data = qm7x.QM7X(
    datapath="QM7X.db",
    raw_data_path="./",
    batch_size=10,
    num_train=10000,
    num_val=10000,
    num_test=10000,
)
data.prepare_data()
print(data)
```

Error:
```
Traceback (most recent call last):
  File "/home/james.smith/apps/qcog-orionis/2025-01-27/qm7x.py", line 3, in <module>
    data = qm7x.QM7X(
           ^^^^^^^^^^
  File "/home/james.smith/apps/schnetpack/src/schnetpack/datasets/qm7x.py", line 237, in __init__
    splitting=splitting or GroupSplit(splitting_key="smiles_id"),
                           ^^^^^^^^^^
NameError: name 'GroupSplit' is not defined
```

# Next Steps

I'm happy to make adjustments, just let me know what you think.

All the best,
James

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7e05fccf3594`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.