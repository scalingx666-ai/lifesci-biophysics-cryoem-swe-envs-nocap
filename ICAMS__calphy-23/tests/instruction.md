# Issue

**Title:** Do not assume all pair_coeff come with a path

## Description

At least for MTP potentials the format for the lammps potential is like

pair_style mlip mlip.ini
pair_coeff * *

where mlip.ini is the potential specific file.  That means when
fix_paths tries to look a filepath to adjust in pair_coeff it won't find
anything and crash.  This also means fix_paths might have to be applied
to pair_style also, but I haven't done that here. :/

I could not test further if there's issues with MTPs, because I'll need to custom build the lammps python first, so I am not sure if this change is the best way about it, but at least this quick fix got me somewhere calphy actually starts to try and run a calculation.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `82ba21162d38`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.