# Issue

**Title:** Keep custom GB parameters when extracting and injecting them

## Description

extract_parameters! wrote the default per-element GB values and inject_interaction rebuilt the interaction from them, so values given when setting up a system were lost, and for GBn2 the nucleic acid parameters and neck tables were reset as well. The GB interactions now store the values they were built with and use them in both functions.

This fix keeps the setup values in the interaction so that extraction and injection can use them. A more robust option would be to store the per-element parameters themselves in the GB interactions and compute the per-atom arrays from them, so that the generic parameter mechanism could be used and extraction and injection would not need to be written separately.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `32843dcd787e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.