# Issue

**Title:** Fix all doc warnings

## Description

Fix all the outstanding doc warnings, broken links, and DeprecationWarning, *all of them*

Fixed missing citation links
Docs now build on Sphinx 1.[567].X (more of a pain than I would like, cc @jchodera)

I don't think we actually use anything in the shipped `notebook_sphinxext`
file and is a leftover things from when we initially copied the docs
from MDTraj, so we might want to remove this in the future.

To fix the citations, I had to comment out many unused citations to get the warnings to stop, we can uncomment them if we actually cite them.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7ddc5ec3f132`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.