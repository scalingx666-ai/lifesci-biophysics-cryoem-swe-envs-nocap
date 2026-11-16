# Issue

**Title:** Remove unused NxN matrix in OBC calculations

## Description

While looking through the OBC implicit solvent implementation I noticed that born_radii_and_grad allocates a zeros N×N matrix on every step that is never written to. This PR removes it and returns nothing instead, dispatching to a new CPU and GPU code path that skips the unused term.

I am aware that this duplicates quite a bit of code, but I wasn't sure how you'd prefer to handle that.
There is a small benchmark I just used to check if what I did actually has an effect, it can be removed if it is not necessary.
I don't know how much this actually affects real simulation performance, possibly the difference will be negligible.

Before
```
n_atoms   alloc (bytes)   median (ms)
------------------------------------
50        51712           0.12
100       181312          0.54
200       682528          2.50
400       2639840         10.21
```

After
```
n_atoms   alloc (bytes)   median (ms)
------------------------------------
50        11472           0.11
100       21136           0.53
200       38840           2.11
400       75712           8.76
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ce53d2bc3c45`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.