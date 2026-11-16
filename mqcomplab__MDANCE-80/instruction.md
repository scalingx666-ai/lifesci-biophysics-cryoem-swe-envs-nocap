# Issue

**Title:** Add mdBIRCH clustering algorithm implementation

## Description

Add mdBIRCH clustering algorithm for molecular dynamics trajectory analysis.

### Changes:
- **Added mdBIRCH implementation** (`src/mdance/cluster/mdbirch.py`): Complete implementation of the mdBIRCH clustering algorithm with RMSD-calibrated merge decisions for online clustering of molecular trajectories
- **Added execution script** (`scripts/mdbirch/run_mdbirch.py`): Script to run mdBIRCH clustering on trajectory data
- **Added test suite** (`tests/test_mdbirch.py`): Comprehensive tests for mdBIRCH

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `028b785d944a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.