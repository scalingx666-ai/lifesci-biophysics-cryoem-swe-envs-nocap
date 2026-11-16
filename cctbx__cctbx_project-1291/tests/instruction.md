# Issue

**Title:** mmtbx.reduce2: Place water H with virvel

## Description

## Problem
With add_h_to_water=True or qmready=True, reduce2 gives every water the
same orientation (#1150). With keep_existing_H=True, a water that already
has H1/H2 gets a second pair and the run stops with duplicate atom labels.

## Changes
- add_h_to_water and qmready place the water H with
  mmtbx.hydrogens.water_protonation (mmtbx.development.virvel), each
  pointing at its H-bond acceptors, once the other H are final.
- add_h_to_water=True also places water H with approach=optimize; with
  approach=remove it is a Sorry. qmready alone adds no water H with
  approach=optimize.
- hd_exchange.add_water_h_parents() enters the water H, with the water O
  as parent, in the H parent table that neutron_option uses.
- A model of only waters and ions has no H before the water H, so it skips
  the Optimizer and gets its water H at the end.
- Version 3.11.0. On 3wu2 (5,966 water H): +5.6 s of 117 s, +37 MB peak
  memory.

Question: for electron microscopy and electron diffraction models, virvel
on its own and mmtbx.model (electron scattering table) use the nuclear
water O-H length, 0.98 A; reduce2 uses 0.85 A unless
use_neutron_distances=True, as for all its H. Should reduce2 switch to
nuclear lengths for electron models, or keep one length setting?

## Tests
- tst_reduce2_water_h.py (new): orientation toward the acceptors, parity
  with water_protonation on the same model, models of only waters and
  ions, several models, input water H, no crystal symmetry, qmready,
  approach=optimize and remove.
- tst_hd_exchange.py: add_water_h_parents() against h_parent_table().
- Full Phenix suite on Linux: no new failures on the branch compared to
  master.

## Coding agent
| Field | Value |
|-------|-------|
| Model and version | Claude Opus 5.5 (`claude-opus-5-5`) |
| Harness and version | Claude Code 2.1.311 |

## Checklist
- [x] Searched open and closed PRs and issues for prior work: #1150
- [x] New or changed tests are registered in the module's `run_tests.py`
- [x] Docstrings and PHIL `.help` strings cover the new or changed behavior
- [x] No new external dependency, or it was agreed with the developers
- [x] No imports of modules outside this repository; `chem_data` and
      `phenix_regression` are used only for data, with a skip when absent
- [x] Interface or breaking changes are called out under Changes
- [x] A human has reviewed the complete diff

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8c12e2d250c2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.