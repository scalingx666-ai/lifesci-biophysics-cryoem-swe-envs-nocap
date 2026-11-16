# Issue

**Title:** Pass parameters to the RDKitConverter from MDAnalysis

## Description

Allows passing parameters to the RDKitConverter of MDAnalysis from within the `run` method of the Fingerprint class:

For example, if your ligand selection is an Mg atom, you have to pass `force=True` to MDAnalysis, which can be done like so:
```python
fp.run(..., converter_kwargs=[{"force": True}, {}])
```

The second empty dict is for the protein selection, meaning that we don't pass any parameter to the converter for that selection.

TODO:
- [x] sphinx not resolving docs links to classes in MDAnalysis

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `430a2fa1148d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.