# Issue

**Title:** iotbx: allow pickling/deepcopy of pdb input with a populated __dict__

## Description

- declare __getstate_manages_dict__ and round-trip the __dict__ in __getstate__/__setstate__ so a populated cache (e.g. _scale_matrix) no longer breaks pickle/deepcopy; preserve the dict rather than drop it (scale_matrix()'s cache can be a deliberate None not recoverable from the source lines)
- add tst_input_pickling.py regression test (pdb + cif, incl. suppressed-None)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4f9a65a3e814`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.