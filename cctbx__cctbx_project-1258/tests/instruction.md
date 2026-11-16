# Issue

**Title:** probe2: generate interaction dots in C++

## Description

`ContactDB` can now also keep every reported dot, classified and placed with
probe2.py's arithmetic, and `_generate_interaction_dots()` builds its results
from those dots in place of the Python loop. Kinemage output without
`count_dots` or `contact_summary` is written straight from the C++ arrays, so
`run()` returns empty results for it.

`Helpers.getAtomsWithinNBonds` now calls the bond walk ContactDB uses for its
exclusions, so probe2 and reduce2 share one implementation.

`tst_probe2_contact_records.py` checks probe2 output against fingerprints
recorded from the Python loop, one case per format, approach, ContactDB
parameter and kinemage writer option, plus a two-model hierarchy.
`mmtbx.probe2` kinemage on 1aon: 113 to 46 s, with about 40% less peak memory.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `98a28ca81c98`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.