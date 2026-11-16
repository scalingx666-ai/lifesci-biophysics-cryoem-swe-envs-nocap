# Issue

**Title:** cctbx.xfel.merge export batch MTZ for careless

## Description

Define a new "export" worker with cctbx.xfel.merge to consolidate the expt+refl files from all ranks, and write a single MTZ to disk in "batch mtz" format.  The file will have appropriate columns and metadata for careless.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `984c2004b199`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.