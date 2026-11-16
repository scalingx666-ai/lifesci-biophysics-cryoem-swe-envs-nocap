# Issue

**Title:** New calculator

## Description

This is the new calculator of torchmd-net. The idea is to have a way to transform the units of measure of energy and forces to run a molecular dynamic simulation with torchmd that works only with forces in kcal/mol/A. 
- [x] update calculator in torchmd-net
- [ ] add the new argument `output_transform`  in torchmd/run.py when the external module is built. 

Example of yaml file to run the simulation with torchmd using the `output_transform `:
```python 
external:
  module: torchmdnet.calculators
  file: [path to the NNP ckpt]
  output_transform: 'lambda energy, forces: (energy * 23.0609, forces * 23.0609)' # eV->kcal/mol, eV/A->kcal/mol/A
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e02c076aefea`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.