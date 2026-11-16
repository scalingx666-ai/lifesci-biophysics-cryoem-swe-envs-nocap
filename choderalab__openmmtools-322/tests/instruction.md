# Issue

**Title:** Add WaterCluster testsystem

## Description

Creates a few waters in a harmonic restraining potential.

Implementation draws on `WaterBox` and `LennardJonesCluster`.

This was suggested by @jchodera as a testsystem that would allow a smaller number of water molecules than possible in `WaterBox`.

![water_mov](https://user-images.githubusercontent.com/5759036/35070597-c11c75a8-fbab-11e7-8401-ec167b1a3cde.gif)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8c2dffda7fe2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.