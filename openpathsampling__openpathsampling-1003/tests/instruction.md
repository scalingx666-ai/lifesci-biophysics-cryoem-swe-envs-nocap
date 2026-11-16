# Issue

**Title:** Prevent netcdfplus from trying to load netcdfplus CVs as SimStore CVs

## Description

As of #995, it appears that netcdfplus tries to load CVs as SimStore CVs. This is apparently because netcdfplus maps a class name (not fully qualified; just the class name) to a class. And once SimStore CVs were found in the chain of subclasses, netcdfplus replaced the old CVs with the new ones. This would lead to errors when loading old CVs (which do not have the same storage dict as new CVs).

The solution is to exclude classes in the `openpathsampling.experimental.storage` subpackage from being listed as classes that netcdfplus can load.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `523de5fef692`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.