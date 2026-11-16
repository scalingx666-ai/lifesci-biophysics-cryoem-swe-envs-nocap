# Issue

**Title:** Fix probe2 crash on atoms near crystallographic symmetry mates

## Description

## Summary
Fix a probe2 crash on atoms near crystallographic symmetry mates.

When an atom sits near a crystallographic symmetry mate (e.g. GLN NE2 ~2.06 Å from its symmetry copy in 3q9v), the restraints engine expresses some covalent bonds as **ASU bond proxies** rather than simple bond proxies. `Helpers.getBondedNeighborLists()` only processed the simple proxies, so hydrogens such as HE21 were left with no bonded neighbors and probe2 crashed with `Found Hydrogen with no neighbors`.

## Change
- `mmtbx/probe/Helpers.py`: `getBondedNeighborLists()` gains an optional `asuBondProxies=None` argument. When provided, ASU proxies with `j_sym == 0` (same asymmetric unit) are added to the bonded-neighbor lists. Behavior is unchanged when no ASU proxies are passed (purely additive).
- Callers in `mmtbx/programs/probe2.py`, `mmtbx/programs/reduce2.py`, `mmtbx/reduce/Optimizers.py`, and the `Helpers` self-tests now capture the second return value of `get_all_bond_proxies()` (the ASU proxies) and pass it through, instead of discarding it with `[0]`.

## Test
Adds `mmtbx/validation/regression/tst_probe2_asu_bond_proxies.py`, which exercises the ASU-proxy path. Passes locally.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cc3ad6f156f7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.