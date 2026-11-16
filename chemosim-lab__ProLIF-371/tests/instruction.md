# Issue

**Title:** Handle fragmented residue issue

## Description

Fixes issue #366 

When a residue is fragmented into several fragments (typically from a bond detection issue), ProLIF used to silently allow it and only kept the last fragment as the sole member of the residue (most often a single hydrogen atom).
It will now default to raising a detailed error.

The behaviour can be controlled by setting `prolif.exceptions.FragmentedResidueError.on_error` to `warn/raise/skip` or a callable that handles the error message and other args directly.

This last option could be use to recombine fragmented residues on the fly, a simplistic example is shown in `test_molecule.py:test_disconnected_residue_custom_callback`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `60f0126a5e4f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.