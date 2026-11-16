# Issue

**Title:** Fix reduce2 His flip breaking user-defined bonds

## Description

.

## Problem

reduce2's `Optimizer` decides Asn/Gln/His flips (and the ion "lock-down" that suppresses them) from local geometry and element type only — a spatial neighbour search plus `element_is_positive_ion()`. It never consults the model's bond restraints, so a user-defined (custom) bond on a flipping atom is invisible to the flip logic and can be silently broken.

The Histidine ring is the exposed case: `MoverHisFlip` validates the ring nitrogens ND1/NE2 by counting hydrogen (`== 1`) and carbon (`== 2`) neighbours, not their total number of bonds, so an extra bond — to a metal, or to another partner — slips through, the flip Mover is built, and the ring can be flipped into an orientation that makes the bond geometrically impossible. (The ring carbons use strict neighbour-count checks and are already protected; the Asn/Gln amide is protected because `MoverAmideFlip` raises on an unexpected neighbour count.)

## Fix

`_PlaceMovers` now skips the His flip when a ring nitrogen carries a bond outside the standard ring connectivity, logging `Did not add MoverHisFlip … (bond outside standard ring connectivity)`.

The check runs **after** the ion lock-down (`bondedConfig`), so a genuinely metal-coordinated histidine found by the spatial ion search is still locked in place, not skipped. Verified on 1xso: the full set of His flip/lock decisions is byte-identical before and after this change.

Also rewords the amide message `Could not add` → `Did not add MoverAmideFlip` (intentional skip, not an error).

## Tests

`mmtbx/regression/tst_reduce_custom_bond.py` (now registered in `run_tests.py`):
- **control** — the histidine is flippable when nothing unusual is bonded;
- **bug** — a user-defined ND1–SG bond suppresses the flip;
- **no-regression** — a genuinely Zn-coordinated His (His 69 + Zn from 1xso) stays locked; this fails if the bonded-neighbour check is placed before the lock-down instead of after it.

Verified locally: the new tests, `tst_reduce.py`, and `tst_reduce_switch.py` all pass; the full-1xso reduce2 flip decisions are unchanged; `find_clutter` clean.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `27cfb6e456c3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.