# Issue

**Title:** dynamic langevin

## Description

# **Summary** (Briefly summarize the purpose of this pull request)

Fix #1635 by adding dynamically selected spatial regions to `ensemble heat_lan`.

# **Modification** (Describe the main changes made in this pull request)

This PR extends `ensemble heat_lan` to support dynamically selected Langevin heat-source and heat-sink regions based on the current atomic positions.

The existing group-based syntax is kept unchanged. A new region-based form is added, where the source and sink are defined using fractional coordinates along the three simulation-cell vectors.

This allows atoms to enter or leave the thermostatted regions during the simulation, without introducing dynamic groups or changing the existing grouping infrastructure.

## New syntax

The original group-based syntax is unchanged:

```text
ensemble heat_lan T T_coup delta_T label_source label_sink
```

A new region-based syntax is added:

```
ensemble heat_lan T T_coup delta_T a_min_source a_max_source b_min_source b_max_source c_min_source c_max_source a_min_sink a_max_sink b_min_sink b_max_sink c_min_sink c_max_sink
```

The regions are defined in fractional coordinates with respect to the current simulation box.

For example,

```
ensemble heat_lan 3000 100 200 0.20 0.30 0 1 0 1 0.70 0.80 0 1 0 1
```

defines

source: 0.20 <= s_a < 0.30
sink: 0.70 <= s_a < 0.80

with no restriction along the b and c box directions.

# **Licensing** (Please read and confirm before submitting this pull request)

By submitting this pull request, I agree that my contribution will be included in GPUMD and redistributed under the existing GPUMD license. Newly added source files follow the existing GPUMD license-header style when applicable. No external source code with incompatible licensing has been copied into this pull request.

# **Validation** (Describe how this pull request was tested)

## Regression tests

All tests in `run_tests_zheyong.sh` pass, including one using `heat_lan`.

## New test

```
potential   Si_2022_NEP4_3body.txt
velocity    3000 seed 12345

time_step   2

# Melt and equilibrate liquid Si at 3000 K and 0 GPa.
ensemble    npt_scr 3000 3000 100 0 50 1000
dump_thermo 100
dump_restart 20000
run         20000

# Test the dynamic-region Langevin heat source and sink.
# Fractional-coordinate regions along the box vectors:
# source: 0.20 <= s_a < 0.30, full b and c directions (3200 K)
# sink:   0.70 <= s_a < 0.80, full b and c directions (2800 K)
ensemble    heat_lan 3000 100 200 0.20 0.30 0 1 0 1 0.70 0.80 0 1 0 1
compute     0 10 100 temperature
dump_thermo 100
dump_xyz     1000 dump.xyz velocity
dump_restart 20000
run         20000
```

The trajectory gives an average source temperature of about 3200 K and an average sink temperature of about 2830 K, consistent with the target temperatures of 3200 K and 2800 K.

<img width="1200" height="800" alt="region_temperature_profile" src="https://github.com/user-attachments/assets/ee1ccd21-2804-42c0-8ebe-032aeb93f66e" />




**Others**

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a7c1f3a60b6b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.