# Issue

**Title:** missing FFTW2_LIBS in MoleculesToTriangles/CXXSurface/Makefile.am

## Description

building coot using a clipper without fftw2, e.g. from the openSUSE:Science project, reveals a missing FFTW2_LIBS in the link rule of MoleculesToTriangles/CXXSurface/Makefile.am

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `105d3bfba389`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.