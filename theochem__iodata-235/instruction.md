# Issue

**Title:** Simple speedup

## Description

@BradenDKelly Here is another small change, i.e. the real fun as you mentioned, with a minor noticeable improvement in performance.

Test file: [gaussian.zip](https://github.com/theochem/iodata/files/6197091/gaussian.zip)

Before:

```
   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.001    0.001    0.072    0.072 overlap.py:68(<listcomp>)
        1    0.000    0.000    0.001    0.001 overlap.py:71(<listcomp>)
        1    1.426    1.426    5.128    5.128 overlap.py:33(compute_overlap)
        1    0.000    0.000    0.000    0.000 overlap.py:163(<listcomp>)
        1    0.000    0.000    0.000    0.000 overlap.py:164(<listcomp>)
        1    0.000    0.000    0.000    0.000 overlap.py:154(__init__)
   725220    2.705    0.000    2.705    0.000 overlap.py:168(compute_overlap_gaussian_1d)
      882    0.006    0.000    0.061    0.000 overlap.py:207(gob_cart_normalization)
      202    0.002    0.000    0.071    0.000 overlap.py:181(_compute_cart_shell_normalizations)
```

After:
```
   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.001    0.001    0.068    0.068 overlap.py:68(<listcomp>)
        1    0.000    0.000    0.001    0.001 overlap.py:71(<listcomp>)
        1    1.217    1.217    4.896    4.896 overlap.py:33(compute_overlap)
        1    0.000    0.000    0.000    0.000 overlap.py:166(<listcomp>)
        1    0.000    0.000    0.000    0.000 overlap.py:167(<listcomp>)
        1    0.000    0.000    0.000    0.000 overlap.py:157(__init__)
   725220    2.745    0.000    2.745    0.000 overlap.py:171(compute_overlap_gaussian_1d)
      202    0.002    0.000    0.067    0.000 overlap.py:184(_compute_cart_shell_normalizations)
      882    0.006    0.000    0.057    0.000 overlap.py:210(gob_cart_normalization)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `270997707b66`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.