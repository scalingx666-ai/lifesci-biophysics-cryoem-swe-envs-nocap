# Issue

**Title:** Fix reaction methods cylinder constraint docstring

## Description

The class docstring of ReactionAlgorithm documented the radius parameter
of set_cylindrical_constraint_in_z_direction as 'radius_of_cylinder',
but the script interface (ReactionAlgorithm.cpp) reads the key 'radius'.
A user following the documentation deterministically hit
RuntimeError: Parameter 'radius' is missing.

Rename the documented parameter to 'radius' to match the script interface
and the testsuite. Add a contract test that parses the documented
parameter names from the docstring and asserts the script interface
accepts them.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bc2f57b25658`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.