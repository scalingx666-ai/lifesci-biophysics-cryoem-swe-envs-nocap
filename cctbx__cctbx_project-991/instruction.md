# Issue

**Title:** Mpi helper counter

## Description

I was working on a custom `cctbx.xfel` worker and found myself `gather`ing + `count`ing objects multiple times. I realized that `mpi_helper` could just as well use a simple `count` method that returns a [`Counter` object](https://docs.python.org/3/library/collections.html#collections.Counter) (a dict-like that automatically counts every occurrence of input list or dict). 

While at it, I realized that the mpi_helper methods could be simplified; in particular, the `flex_type` can be implicitly learned from `flex_array` – as long as the `root` rank has a not-None `flex_array`. Consequently, I allowed the `flex_type` to be inherited from `flex_array` if left undefined.

Initially, I wanted to simplify all instances where `cumulative_flex` and `aggregate_flex` use "redundant" `flex_type`, but since it works in case of completely empty input, I left it as it. After all, _[Explicit is better than implicit](https://peps.python.org/pep-0020/)_.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d3345800bb25`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.