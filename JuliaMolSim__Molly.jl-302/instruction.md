# Issue

**Title:** Add TSS state loggers. Needed for FFRefine

## Description

# Add a per-sample logging hook to TSSSimulation

System loggers only see the physical System. Some analyses need what TSS knows about each sample at the moment it is drawn: the active state, the active window, the reduced potentials of the window states, the estimator's log normalization and the conditional state weights. Examples are building a replay archive for MBAR reweighting, or checking estimator diagnostics offline. That information exists only inside the TSS cycle loop, so it can't be rebuilt from a system logger afterwards. In particular, a system logger can't recover the active window, nor the active state in temperature TSS.

This PR adds a generic hook that exposes that information. It also removes some stale code that was left behind in a previous refactor.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `40968654ea54`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.