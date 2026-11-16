# Issue

**Title:** Support custom columns in XYZ files

## Description

.

Some other notable points:
- The PR includes an example of this feature for XYZ files.
- Exceptions were added to `.pylintrc` due to false-positives, which seem fairly general. Mipy should be used to catch such problems, as explained in #101.
- The documentation page generator `gen_formats.py` was extended (and simplified) to list the optional arguments supported by each file format.
- This is not API breaking, but rather an extension.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `66c0553b3b18`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.