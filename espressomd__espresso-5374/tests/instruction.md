# Issue

**Title:** Fix fractal_dimension argument parsing

## Description

The Cluster docstring advertised fractal_dimension(dr=None), but dr is a
mandatory parameter (the histogram bin increment) and no layer materializes
a default. Calling fractal_dimension() with no argument made the script
interface do an unconditional parameters.at("dr"), throwing an opaque
std::out_of_range that surfaced to Python as 'IndexError: unordered_map::at'.

Fix:
- Cluster.hpp: use the named get_value overload get_value<double>(parameters,
  "dr") so an omitted dr raises the standard, friendly
  Exception("Parameter 'dr' is missing.") instead of an opaque
  std::out_of_range.
- cluster_analysis.py: correct the docstring signature to
  fractal_dimension(dr), removing the false promise of an optional/None
  default.

Adds a regression test (test_fractal_dimension_requires_dr) asserting that
omitting dr raises the friendly 'Parameter dr is missing' error and that
dr=None raises a clear 'not convertible to double' conversion error.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `519e32925f28`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.