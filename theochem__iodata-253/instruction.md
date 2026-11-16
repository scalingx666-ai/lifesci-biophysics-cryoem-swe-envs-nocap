# Issue

**Title:** Make atom lines configurable in the input writer

## Description

.

Few other things are also fixed in this PR:
- Catch warning in `test_input_orca_from_molden`
- Remove unused `kwdocs` from `document_write_input`
- More code reuse between input writers (`write_input_base` instead of `populate_fields`)
- Conversion of atomic coordinates to angstroms is moved to program-specific code. The main motivation is to postpone such conversions as long as possible.

I considered a few ways to implement this, e.g. to pass in `atom_line` as a template string. Unfortunately, the `.format` method is not as versatile as the `f` prefix for string literals, which makes it impossible to get the same functionality with atom_line string templates. Hence, `atom_line` is a callable.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8712d41b7877`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.