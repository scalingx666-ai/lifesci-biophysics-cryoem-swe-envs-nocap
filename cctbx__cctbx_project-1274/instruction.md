# Issue

**Title:** Reduce2: near-acceptor sampling for single-H rotators, with a safegua…

## Description

…rd for singletons

Restores the near-acceptor candidates and the wider fine search that Reduce's RotDonor.cpp uses for single-H rotators (OH/SH), lost when the coarse grid was pruned in #931. Where an OH or SH points at a close acceptor, the direct line bumps (more so with neutron X-H lengths) and the good pose lies 10-30 deg off it; today it is never offered. Example: 7PRG FUC O3 in hd_and_h ends between two Ca (score 6.3) instead of H-bonded to Asp99 (16.6).

Changes
- Movers.py: Reduce's +-15/+-30 deg offsets after today's candidates (order and near-duplicates kept); fine range -10..+9 deg for single-H rotators through a new optional fineRange (default unchanged for other rotators); NumPrimaryCoarsePositions(); NominalXHBondLength() shared with the acceptor selection in Optimizers.py (today's == "S" test kept).
- Optimizers.cpp/.h: refineBestPrimary(), run only when a Mover's coarse winner is not a primary candidate: both anchors and their fine poses scored fresh, switch only on a strictly higher score, state set through setMoverState; per-anchor dot-cache keys for fine poses. GetExcludedDotsSize(), a read-only diagnostic used to measure the dot cache (happy to drop it).
- Not changed, but noted: the existing fine step accepts a pose only if it beats m_highScores, which can be stale once neighbours have moved (1r27 Thr A 235 is a case R+ now resolves).

Results (57 models, X-ray and hd_and_h): rescored totals higher in every run (e.g. 1r27 +90 / +330, 7PRG +56 / +170); no singleton lower than before; a few coupled cliques lower by 0.01-1.15 (6tte Thr/Ser now H-bond to each other and a water instead of Thr HG1 pointing at Trp CE2; 6KK8 same partners, shifted). Too-short H-bond dots go down.

Cost: +2.5% wall time over the set (7PRG up to +9%; the 1r27 fine step doubles), peak memory up to +144 MB, mostly the trimmed-dot cache. The estimated state enumeration of some cliques grows a lot (2v3g largest clique 1,341 -> 20,456 states); no run came near our 2 GB limit, but it is worth watching.

Tests: tst_reduce2_near_acceptor.py (each test shown to catch planted bugs, Python and C++); tst_add_hydrogen_12 answers updated. All reduce2/hydrogens suites pass.

Not covered: coupled Movers (coarse-then-fine limits in cliques), and the 45-degree spacing of the remaining grid; both for a later step.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b590ddf283a0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.