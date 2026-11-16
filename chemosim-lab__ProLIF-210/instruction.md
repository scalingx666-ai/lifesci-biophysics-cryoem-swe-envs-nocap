# Issue

**Title:** Add interactions iterator

## Description

Adds custom `.interactions()` iterator for values in `fp.ifp` that allows doing:
```python
for index, ifp in fp.ifp.items():
    for interaction in ifp.interactions():
        print(interaction.ligand, interaction.protein, interaction.name, interaction.metadata)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9be3cea70f0f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.