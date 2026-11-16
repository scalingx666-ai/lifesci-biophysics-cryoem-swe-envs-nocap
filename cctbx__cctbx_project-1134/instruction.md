# Issue

**Title:** XFEL CI and conda improvements

## Description

- Remove gemmi from psana_environment.yml. The conda solver does not like the combination of psana and gemmi for some reason. This change decreases solving times from 30 min to <1 min. Dependencies of gemmi are minimal so it is fine to add it subsequently with `--no-deps`.
- A post-install script in the conda environment looks for `fi_info`, which is related to libfabric, an HPC networking framework. The script is for gdk-pixbuf, an image library with no HPC importance, so this may be something accidentally inherited from their conda recipe. Libfabric is not present in the rockylinux minimal image so we just put a dummy fi_info executable in the PATH to let the post-install script complete.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `038d01d93fb6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.