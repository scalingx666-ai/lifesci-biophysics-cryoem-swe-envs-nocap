# Issue

**Title:** Log wall time per frame

## Description

Adds the following fields to the statistics logged during HREX simulations:

* wall time per frame for the last batch of frames
* wall time per frame, cumulative average
* estimated wall time remaining (computed from cumulative average time per frame)

Also fixes a minor formatting inconsistency (space after `Frames` in header)

Example output:

```
Frame 90
Wall time per frame, current   : 0.60 s
Wall time per frame, average   : 0.61 s
Estimated wall time remaining  : 554.94 s
HREX acceptance rates, current :  41.9% |  55.8% |  47.0% |  60.4% |  21.0% |  43.8% |  30.5% |  31.8% |  56.3% |  40.1% |  47.3% |  63.4% |  43.8% |  30.5% |  27.3% |  68.6% |  23.0% |  36.8% |  30.3% |  68.3% |  79.6% |  78.9% |  67.2% |  24.0% |  51.8% |  71.6% |   4.2% |  63.3% |  51.0% |  29.4% |  57.2% |  65.5% |  22.0% |  74.6% |  33.7% |  20.8% |  34.6% |  21.5% |  80.8% |  76.7% |  57.4% |  55.3% |  36.7% |  78.1% |  10.2%
HREX acceptance rates, average :  37.5% |  34.8% |  61.0% |  47.1% |  30.9% |  33.8% |  39.4% |  43.1% |  51.3% |  55.5% |  54.8% |  55.5% |  53.5% |  59.5% |  47.5% |  48.5% |  51.1% |  46.6% |  46.3% |  46.0% |  52.8% |  55.9% |  60.2% |  62.4% |  52.4% |  38.3% |  47.9% |  38.2% |  48.0% |  44.5% |  50.8% |  53.4% |  46.5% |  50.5% |  49.2% |  46.9% |  43.0% |  35.1% |  47.0% |  55.3% |  44.2% |  64.0% |  63.9% |  58.7% |  42.9%
HREX replica permutation       : [0, 3, 17, 4, 2, 18, 1, 10, 6, 8, 14, 23, 7, 16, 15, 11, 19, 13, 12, 24, 9, 21, 28, 20, 25, 32, 22, 29, 30, 35, 31, 34, 36, 33, 5, 37, 26, 27, 45, 39, 44, 43, 41, 42, 40, 38]
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c63bc931cc57`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.