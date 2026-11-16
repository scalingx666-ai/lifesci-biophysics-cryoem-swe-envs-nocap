# Issue

**Title:** selection syntax to support parenthesis and "or" shortcut

## Description

With this PR the selection syntax supports parenthesis and multiple "names" as a shortcut for "or" clauses, such as:

```julia
collectatoms(struct, sel"resname ARG GLU ASP")
collectatoms(struct, sel"(serial > 100 and resname ARG) or (serial < 100 and resname GLU)")
```

Disclaimer: I got a lot of help from Gemini AI to do this. After some manual fine tuning I tested the result carefully against the selection syntax of VMD.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b3d4fd0a2925`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.