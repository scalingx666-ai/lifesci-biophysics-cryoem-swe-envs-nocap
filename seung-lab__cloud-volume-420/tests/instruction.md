# Issue

**Title:** feat(uviewer): support displaying bigint segmentation

## Description

It seems that FireFox and Chrome both support BigUint64Arrays now.
It's a little tricky because BigInt and Number do not mix so casting
has to be done carefully.

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `085f69a4644b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.