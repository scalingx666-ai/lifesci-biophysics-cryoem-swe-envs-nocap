# Issue

**Title:** CIF: handle all optional _atom_site fields

## Description

This is a , ensuring that all optional fields in the in
the `_atom_site` category are handled correctly. This also adds tests
for files written by two external tools.

I've taken the liberty to bump the patch number as well.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bbb83543f4e0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.