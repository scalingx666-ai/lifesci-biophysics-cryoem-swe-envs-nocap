# Issue

**Title:** Extract data on harmonic bonds

## Description

This extracts more data from ff14SB.

I confess I don't understand the atom notations: what are, e.g., `CX`, `CT`, `2C`, `C*`, etc? I'm guessing some of these are particular carbons, which accounts for why there doesn't seem to be a `N-Cα` bond listed. Is there special significance to the `protein-` part of the label? EDIT: oh, wait, the type-tag appears in the residue data...I still wouldn't mind understanding the designation scheme, but that resolves the immediate concern. Are there special rules at the N- and C-termini?

Also, is there a rule for key-ordering? In most cases it seems alphabetical, but then you get down to `("protein-H1", "protein-2C")` and that goes out the window. It would be nice to know if there is a rule so one doesn't have to try both orders.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a507527401dd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.