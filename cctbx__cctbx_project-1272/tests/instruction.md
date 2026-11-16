# Issue

**Title:** tst_probe2_contact_records: freeze the H in the dot fingerprint model

## Description

`exercise_dots` checks that probe2's C++ dots match the Python loop they replaced, but it placed H with reduce2 on every run, so any change to H placement broke it (it fails on master now, after the water added in 3140762ade and recent reduce2 changes). It now runs on the hydrogenated model stored in the test, plus a water H-bonded to Val 26 N-H so H-bond dots are covered, and a failure lists every changed case. The new fingerprints come from the Python dot loop before 1e06f034ee; the C++ output matches them in all 24 cases.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `063d29250a9a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.