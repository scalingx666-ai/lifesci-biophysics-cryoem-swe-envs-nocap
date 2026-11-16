# Issue

**Title:** Niggli reduction c++ port

## Description

This PR replaces the existing Niggli reduction with a c++ implementation. This PR ports the cctbx Niggli from python to c++ and was done with Claude Code. A new comphrensive test cross-check the public methods of the new c++ implementation with the existing python implementation.

During serial data reduction, when multiple lattices are present on an image, indexing is performed by repetitive indexing attempts with subsampled observations. Profiling showed the existing Niggli Reduction contributed a significant amount of execution time. This PR reduces the fraction of time in the Niggli reduction from 27.4% to 4% in one test case and from 34% to 3.4% in another.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `71b8fc5255ab`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.