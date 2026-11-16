# Issue

**Title:** Read SDF V2000 files with fixed-width fields

## Description

The SDF loader splits lines at whitespace, but V2000 has fixed-width fields that can touch each other. This breaks for ordinary files:

- the counts line of a molecule with 100 or more atoms and 100 or more bonds, e.g. `120119  0  0 ...`
- bond lines with atom indices of 100 or more, e.g. `100101  1  0 ...`
- coordinates of -1000 or lower, e.g. `2756.8569-2747.9281`

The dumper already writes these fields at fixed widths, so iodata could not read back its own files. For example, dumping a 120 atom chain and loading it again raises an error.

This reads the counts, coordinates, symbol and bond fields by column, as in the V2000 description. The existing sample files already follow it.

The new test dumps a 120 atom chain with coordinates up to +-2000, checks that the touching fields are really in the file, and loads it back. It fails on the current loader and passes with this change. The full test suite passes (`pytest iodata`), and `ruff check` and `ruff format` are clean.

I found this with a round trip test of the dumpers and loaders. I used an LLM assistant (Claude Code) for the investigation and the change, and I checked the results.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `785cf84e7814`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.