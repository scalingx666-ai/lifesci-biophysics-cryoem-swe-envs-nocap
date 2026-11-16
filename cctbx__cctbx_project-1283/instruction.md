# Issue

**Title:** Reduce2: optional symmetry copies as a fixed environment of the Optim…

## Description

…izer

## Problem
reduce2 optimizes H against the model's own atoms only, so H at crystal
contacts ignore the symmetry mates: H-bonds to a mate are missed and clashes
with it go unseen (e.g. 1us0 Gln A 49 overlaps Gln A 283 HA instead of
H-bonding Thr A 287 OG1 of a mate; 1xmk His A 354 is not locked by the ion
rule for the Cd in a mate). #1276 adds the copies to the model and reprocesses
it; in our tests that broke approach=optimize, left a stale riding-H manager,
lost metal-coordination restraints and let copies end in other states than
their originals.

## Why copies as a fixed environment
#1276 adds the symmetry copies to the model as Movers. To test it on current
master we first had to adapt it to the recent reduce2 changes, which gave a
repaired version of copies as Movers. In parallel we tried a second design:
the copies as a fixed environment that the Optimizer scores against but never
moves, refreshed until they match their originals. We compared both on several
models.

| Model | Change at crystal contacts vs master | Copies as Movers vs copies fixed |
|---|---|---|
| 5C11 | 1 clash removed | same |
| 1us0 | 4 H-bonds gained, 1 lost | same |
| 6vw1 | 2 H-bonds gained | same |
| 1r27 | 2 H-bonds gained, 2 lost, 2 clashes removed | same |
| 7PRG | 4 H-bonds gained, 1 clash removed (neutron: 8 gained, 2 clashes removed) | same |
| 1us0, approach=optimize | 1 H-bond gained, 1 lost | same |

With the same results, copies as a fixed environment is the better choice:
- Copies stay identical to their originals. As Movers, many copies end in a
  different state than their own original (up to 29 of 61), which cannot be
  physically right; making them agree means freezing them, which is this
  approach.
- The model is never changed or reprocessed, so its restraints (including metal
  coordination) and riding H stay as placement made them.
- It is faster: about +10-14% wall time over master, against +46-65% for copies
  as Movers.

## Changes
- New option include_symmetric_copies (default False). Copies of the symmetry
  mates within 3.6 A (sym_equiv_residue_groups(), the input's own symmetry;
  none for a placeholder cell or without symmetry) are a fixed environment of
  the Optimizer: scored, never moved, never Movers; Mover placement sees them
  (His ion lock, single-H acceptor candidates).
- Bonds across symmetry mapped with their operators; atoms left out at special
  positions resolve to the atom kept at that site, in both directions.
- Refresh: after each alternate, the copies follow their originals and only the
  Cliques near changed copies are optimized again (at most 3 passes); Movers
  still changing keep the best-scoring combination of their states with
  matching copies; the Movers a refresh affects are reported with fresh
  scores; after the time limit no further refresh pass starts, and a pass the
  deadline overtakes (part of it solved as singletons, as the main pass since
  #1282) keeps the best combination of its Movers' previous and new states,
  rescored with matching copies, so it never ends below the accepted
  solution.
- The model is never reprocessed; option off: output byte-identical.
- Interface: Optimizer(..., symmetryCopies=None) (other callers unchanged);
  getSymmetryCopies(), getSymmetryCopiesSummary(). Clique optimization (as
  after #1281/#1282) moved unchanged into _OptimizeCliques()/_OptimizeFine().
- Limits: the model's own water phantoms are placed before the copies exist;
  flipkin and comparison passes run without copies; Movers outside the
  refresh-affected Cliques report their stored search scores, which can be
  stale in multi-Mover Cliques, as on master (a separate issue).

## Tests
- New mmtbx/regression/tst_reduce2_symmetry_copies.py (9 tests, ~9 s),
  registered in mmtbx/run_tests.py; each mapping, placement and refresh test
  fails with its part reverted.
- macOS: the 28 reduce2 test files in mmtbx/run_tests.py pass; with the option
  forced on, the 27 existing ones pass (77 runs built copies).
- Option off: written models byte-identical to master (6678bb2805) on 12 runs (5C11, 1xmk,
  1us0, 1us0 optimize, 6vw1, 1r27, 5gsk, 7PRG, 7PRG hd_and_h, 3j4p, 8afw, 4wsc).
  Option on: every copy matches its original at the end; bond proxies of the
  model unchanged; H-bonds gained at crystal contacts (e.g. 1us0 +4/-1);
  1xmk His A 354 locked through the Cd copy; wall time +0-16% over master
  (1us0 8.9 -> 10.0 s, 1r27 35.9 -> 40.4 s, 4wsc 50.1 -> 56.1 s).

## Coding agent
| Field | Value |
|-------|-------|
| Model and version | Opus 5.5 |
| Harness and version | Claude Code, VS Code extension (version) |
Planning and review in a Claude chat (Claude Desktop App Opus 5.5 high effort); external review of the diff by Codex GPT-6 Astra.

## Checklist
- [x] Prior work: #1276 (this replaces its approach, keeping the option name)
- [x] New test registered in mmtbx/run_tests.py
- [x] PHIL .help and docstrings cover the new behavior
- [x] No new external dependency
- [x] No imports from outside this repository
- [x] Interface changes listed under Changes
- [x] A human has reviewed the complete diff

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `beacbf9edc9c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.