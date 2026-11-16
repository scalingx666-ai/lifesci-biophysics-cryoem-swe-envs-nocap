# Issue

**Title:** msifrc2lt.py: fix NameError in the cvff out_of_plane branch

## Description

Thanks for the kind reply in #120, and no disappointment at all. This is the small change from the issue:

- line 2327: use `K` (read just above) instead of the undefined `Kchi`
- line 2791: the assert now checks `dihedral_atom_name[0]` instead of the undefined `ange_atom_name[0]`

I checked that the file compiles, but I don't have a `.frc` file, so I could not run the converter itself.

I'm working on [physverify](https://github.com/rcgiri-physics/physverify), a tool for testing physics code, and I'm testing it on public projects with help from an AI assistant. That's how this came up.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e0369a95cef0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.