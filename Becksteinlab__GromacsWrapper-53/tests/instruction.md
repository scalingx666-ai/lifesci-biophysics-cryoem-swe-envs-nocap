# Issue

**Title:** Updated config to be compatible with Gromacs 5.1.1

## Description

Since Gromacs 5.1.x removed the use of g_\* methods, instead opting only
for the new `gmx <command>` style, this config features the necessary
bits to use the commands this way. This configuration will _not_ work
for Gromacs 4.x or earlier. We will hopefully replace this config style
with one that works for multiple versions more flexibly with #49.

Removed `extras`, as these tools are no longer maintained and it's not
clear that they will work as expected under this config.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `17b59ea27227`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.