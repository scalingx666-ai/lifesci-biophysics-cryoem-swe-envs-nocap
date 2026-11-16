# Issue

**Title:** Remove stray shebangs and normalize CRLF line endings in unit/ modules

## Description

These openmm.unit and amber_file_parser modules are library code, never meant to be run directly, but eleven of them carried a "#!/bin/env python" shebang despite shipping without the executable bit set. doctests.py additionally had Windows-style CRLF line endings throughout the file, unlike the rest of the codebase. Neither affects Python import/execution, but both are packaging-lint failures for any downstream (e.g. rpmlint's non-executable-script and wrong-script-end-of-line-encoding checks) and are simple to fix at the source.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1d716de3b510`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.