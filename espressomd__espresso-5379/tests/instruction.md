# Issue

**Title:** utils: fully reset RunningAverage in clear()

## Description

RunningAverage::clear() only reset the sample count m_n, leaving the
running average, variance, and min/max accumulators stale. After a
clear() followed by re-sampling, var() folded the pre-clear variance
into the new data and min()/max() returned the pre-clear extrema,
silently corrupting a reused accumulator.

Reset all members in clear() so it mirrors the constructor's clean
state. Add a clear_then_resample unit test that re-samples after
clear() and checks avg/var/min/max.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c763ec89eda1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.