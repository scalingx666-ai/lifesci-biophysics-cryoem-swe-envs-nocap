# Issue

**Title:** PythonForce can be restricted to a subset of particles

## Description

This adds a new feature to PythonForce: you can provide a list of particles to apply it to.  That means it only needs to download the coordinates for those particles, and only needs to upload the forces on those particles.  When dealing with a small part of a large system, that can make a big difference.

This is to fix a performance regression in OpenMM-ML that came from switching to PythonForce.  An extreme example is found in the ML tutorial, which uses AceFF 2.0 (one of the fastest ML potentials we support) to simulate alanine dipeptide (22 atoms) both in vacuum and in a box of MM water.  It takes about three minutes for the vacuum simulation and about ten minutes for the water simulation.  The difference is almost entirely from communication overhead: the MM forces take negligible time compared to the ML forces.  Using this feature, the water simulation takes barely any longer than the vacuum one.

The difference won't always be that large.  It depends on how fast the ML potential is and on what fraction of the system you're using it for.  Using orb-v3-conservative-omol, a much more expensive potential, it takes about 24 minutes for the vacuum simulation and 25 minutes for the water one.

Of course, this will require changes to OpenMM-ML to use this feature.  And we can't make those changes until OpenMM 8.6 is released.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5d8d5874b969`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.