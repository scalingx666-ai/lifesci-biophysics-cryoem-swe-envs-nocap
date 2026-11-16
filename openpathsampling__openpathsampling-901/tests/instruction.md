# Issue

**Title:** Add GromacsEngine to docs

## Description

We've had the Gromacs engine working in OPS since version 1.1, and the docstrings were in place and there was an example in the `examples/` directory. However, none of this stuff had made it to the website. This PR puts it there.

While I'm at it, I'm going to try to clean up a few complaints that I see coming from sphinx.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `79ca42f04cb1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.