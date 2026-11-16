# Issue

**Title:** reaction_methods: fix order_n exclusion-range check on multiple ranks (bug-sweep #7)

## Description

The exhaustive O(N) exclusion-range check (search_algorithm="order_n",
the default) ran its distance loop only on the rank that owns the
inserted particle and resolved every candidate via
cell_structure.get_local_particle(), which returns nullptr for a
particle that is neither local nor a ghost on that rank. A candidate
particle owned by a different rank and farther away than the ghost-layer
width was therefore never distance-tested. Only a single boolean was
shipped from the owner to rank 0 and broadcast, with no OR-reduction of
per-rank partial results, so cross-rank exclusion-range violations were
silently accepted, corrupting reaction/MC ensembles on >1 rank.

Fix (HANDLE, not throw, since order_n is the documented exhaustive
fallback meant to work for exclusion ranges larger than the cell size):
in the order_n branch, broadcast the inserted particle's position from
its owning rank, let every rank distance-test the candidates it owns
(real, non-ghost copies only, to avoid double-counting ghosts), and
combine the partial results with a logical-OR all-reduce. The parallel
(get_short_range_neighbors) branch is already correct and is left
unchanged; the shared distance test is factored into a local lambda.

Adds a multi-rank Boost.Test regression case
(order_n_remote_particle_exclusion_range, run with NUM_PROC 2) that
places two particles on different ranks beyond each other's ghost layer
but within exclusion_range; it fails on the old code (overlap missed)
and passes after the fix, with a negative control to guard against
over-detection. check_exclusion_range is made protected so the existing
Testing::ReactionAlgorithm subclass can expose it (test-enabling only).

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e63c9ab28caf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.