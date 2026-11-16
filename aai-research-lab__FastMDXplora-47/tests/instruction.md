# Issue

**Title:** Fix state discovery and 2D metadynamics analysis

## Description

## Summary
- discover FastMDX-generated simulation/energy.csv in thermodynamics while preserving explicit state_csv and legacy filenames
- add true 2D metadynamics surface analysis from axes/free_energy_kjmol records, including contour plotting and dimensional data export
- make metadynamics reweighting dimension-aware by interpolating all biased COLVAR CVs, sharing simulation-layer periodic handling, and reporting explicit HILLS/COLVAR dimensionality mismatches

## Validation
- PYTHONPATH=/tmp/fastmdx-test-deps:src MPLCONFIGDIR=/tmp/fastmdx-matplotlib /home/victory/miniforge3/envs/xplora/bin/python -m pytest -q tests/test_metad_surface.py tests/test_metad_surface_analysis.py tests/test_metadynamics.py tests/test_reweight.py tests/test_reweighted_averages.py tests/test_report_reweighted.py
- PYTHONPATH=/tmp/fastmdx-test-deps:src MPLCONFIGDIR=/tmp/fastmdx-matplotlib /home/victory/miniforge3/envs/xplora/bin/python -m pytest -q tests/test_analyses_do_not_disturb_each_other.py tests/test_analysis_layer.py tests/test_analysis_scope.py tests/test_concrete_analyses.py tests/test_sub3_analyses.py
- PYTHONPATH=/tmp/fastmdx-test-deps:src /home/victory/miniforge3/envs/xplora/bin/python -m compileall -q src/fastmdxplora/analysis/metad_surface.py src/fastmdxplora/analysis/reweighted_averages.py src/fastmdxplora/simulation/metad_surface.py src/fastmdxplora/simulation/pipeline.py
- git diff --check

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `14d279650ff1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.