# Issue

**Title:** Warn if CVDefinedVolume CVs return iterables

## Description

A recurring issue has been that users create CVs that return length-1 arrays/lists, and then use these CVs to define volumes/interfaces. Because Python/NumPy allow order comparisons on length-1 iterables as if they were scalars, sampling goes fine. However, some aspects of analysis expect the CVs to output scalars, and then users get cryptic error messages at unexpected points.

This PR emits a warning if the CV used by a `CVDefinedVolume` returns an iterable.

. 

- [x] Initial implementation for len-1 arrays
- [x] Warn on any iterable: the CV for a `CVDefinedVolume` should not, in general, be any kind of iterable (although things that aren't orderable will fail later anyway)
- [x] Add tests

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0819cffcd52c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.