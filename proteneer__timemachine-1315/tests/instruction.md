# Issue

**Title:** Adds verify_chiral_consistency_of_core to verify cores quickly

## Description

* Can be slow to construct a SingleTopology object just to determine if the endstates are correct given a certain core, this PR separates out the construction of the end state potentials for the potentials necessary to check the chiral endstates (bonded, chiral atoms and not used currently but also chiral bonds).
* One of the reason this is slow in practice is AM1BCCELF10 (AM1 + Omega), this avoids the need to generate charges
* About 5x faster to call `verify_chiral_consistency_of_core` with `smirnoff_1_1_0_sc.py` as shown in https://gist.github.com/badisa/466f73d6045f16454c16239c2bb11096

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1791077310d9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.