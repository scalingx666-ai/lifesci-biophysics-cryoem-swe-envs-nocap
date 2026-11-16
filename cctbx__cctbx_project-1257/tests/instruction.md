# Issue

**Title:** Reduce2 identical vertex cut

## Description

Reduce2 version 3.5.0 makes the recursive vertex-cut solution always match the brute-force optimization (except in the case of tied scores).

The previous version ignored the impact of subgraph Movers on the Movers in the cut graph, so it could get different answers for the coarse optimization. When this happened, sometimes the overall result (after fine motion) was better, but usually it was worse.

This version recursively tracks all Movers that are in the cut sets around the current subgroup and scores them (but does not move them) along with the Movers that it does move.

This was tested on 7k00, which had different results for a 6-element clique in the brute-force and vertex-cut cases. The new approach produces the same result as the brute-force approach.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9816cb787840`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.