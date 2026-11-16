# Issue

**Title:** [Document] MDAnalysis residue handling in WaterBridge

## Description

### Summary
This addresses the confusion discussed in #299, where users were concerned about duplicated water residues affecting WaterBridge detection. The underlying behavior was confirmed to be correct, but the assumption was undocumented.

By clarifying this in the module docstring, users can better understand:
* why duplicated resid values may appear in raw PDB files
* how MDAnalysis resolves this internally
* and why WaterBridge remains correct without manual deduplication.

### Changes
Added description in `water_bridge.py`

```text
Water-mediated interactions --- :mod:`prolif.interactions.water_bridge`
=======================================================================
.
.
.. versionadded:: 2.1.0

Notes
-----
WaterBridge relies on MDAnalysis residue handling to uniquely identify
individual water molecules.

Due to the 4-digit limitation of residue numbering in the PDB format,
multiple water molecules may appear to share the same residue number
(e.g., ``resid 1917``). MDAnalysis internally guarantees unique residue
identities when parsing PDB files (e.g., by offsetting residue indices).

As a result, WaterBridge correctly treats each water molecule as a
distinct residue, and users do not need to manually deduplicate water
residues when using MDAnalysis-supported formats.

```

happy to modify pr if required

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `095ca7087a0b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.