# Issue

**Title:** Remove performance bottleneck in GPU VTK writers

## Description

Description of changes:
- The gpu lb vtk now uses the correct field to write the vtk 
- The pressure is written to an array before the vtk write process to utilize parallelization during the calculation

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9b5f44704cf7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.