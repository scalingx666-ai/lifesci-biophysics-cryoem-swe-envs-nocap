# Issue

**Title:** Allow the CLI -ignore option accept multiple arguments

## Description

Ignoring multiple residue names was a pain (`-ignore HOH -ignore LIG1 -ignore LIG2 ...`). This makes it possible to provide multiple arguments (`-ignore HOH,LIG1 LIG2 -ignore LIG3`) in pretty much any combination. The option to separate residue names by commas is to keep it in line with the -merge option.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3be317196955`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.