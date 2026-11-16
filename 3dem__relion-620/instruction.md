# Issue

**Title:** rotate helical display from model.star

## Description

This is to see **helical** reconstructions as a side-view when displaying the model.star AND ticking "apply orientation". I find this useful, especially for classifications. 

Note that the changes do not affect helical volumes opened as .mrc, since there's no metadata to detect if it is helical. One might however want to slice the rotated helix if opened as a volume *via* the model.star, but these changes do not cause this behavior.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c6c5c7c24ffe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.