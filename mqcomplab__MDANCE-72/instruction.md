# Issue

**Title:** Add DIVINE clustering and other tools

## Description

DIVINE clustering algorithm

- Divine class for divisive hierarchical clustering
- run_divine.py to execute DIVINE
- assign_labels.py helper script
- output CSVs (labels, scores, MSDs)
- analysis_db.ipynb for post-run analysis
- initial test with test_divine.py

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6ccbc9330321`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.