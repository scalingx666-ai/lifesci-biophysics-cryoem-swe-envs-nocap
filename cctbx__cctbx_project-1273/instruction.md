# Issue

**Title:** clashscore2, undowser2, kinemage: reuse the model clashscore2 already processed

## Description

For single-model input clashscore2 already builds the H-added model with probe2's interpretation parameters and restraints. It now exposes it as `processed_model` (and, with `keep_processed_model=True`, the processed ensemble master), and undowser2 and the kinemage probe dots can run probe2 on it, or a copy, instead of interpreting the same model again. Ensembles write each model's atoms into the master, and the multi-model kinemage builder takes `results_by_model` in place of re-running validators.

Also: undowser2 returns early for a model with no waters, and the kinemage dots honour `use_neutron_distances`, so a nuclear clashscore2 run and its dots agree.

Output is identical; the MolProbity Next kinemage stage on 1aon goes from 391 to 327 s. t96 gave no new failures against master; `tst_probe2_contact_records` fails on master too and is fixed in #1272.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `063d29250a9a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.