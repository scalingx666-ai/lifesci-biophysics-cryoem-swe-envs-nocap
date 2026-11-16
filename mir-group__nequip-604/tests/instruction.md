# Issue

**Title:** Implement ``stable_silu_backward`` and update decomposition table

## Description

I found that my NequIP models were consistently immediately crashing in training when using extra-large model sizes, compile mode and OMol25 data. A lot of debugging and profiling located the ``silu_backward`` as the issue, which was overflowing on the first step – so no gradient clipping, LR reduction etc could fix. 

This is due to the expansion of the sigmoid into ``1 / (1 + exp(-x))`` within the decomposed ``silu_backward``, which causes overflows with large activations it seems, but using the ``torch.sigmoid`` primitive avoids this issue. Implemented here, checked that it trains fine and doesn't affect speeds.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d2787c3c2bdd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.