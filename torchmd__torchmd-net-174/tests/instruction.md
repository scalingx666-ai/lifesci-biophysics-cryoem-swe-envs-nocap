# Issue

**Title:** Enable the Ace loader to return a conformation index

## Description

Enable the Ace loader to return a conformation index

```python
ds = Ace('.', paths='dataset.h5')
for s in ds.sample_iter(mol_ids=True):
    print(s)

Data(y=[1, 1], pos=[12, 3], z=[12], neg_dy=[12, 3], q=0, pq=[12], dp=[3])
...

for s in ds.sample_iter(mol_ids=True):
    print(s)

Data(y=[1, 1], pos=[12, 3], z=[12], neg_dy=[12, 3], q=0, pq=[12], dp=[3], mol_id='C4H5N3', i_conf=10355)
...

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `03d788186c33`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.