# Issue

**Title:** Add R2SCAN-D3 pair dispersion implementation

## Description

**Summary**
Add support for the r2scan functional with D3 dispersion correction in the pair_dispersion_d3 implementation.

**Changes**
Documentation: Updated the supported functionals table to include r2scan with the bj damping function
Implementation: Added r2scan functional mapping and its corresponding D3 parameters
Functional code: 54
D3 damping parameters: a1=0.4948, s8=0.7898, a2=5.7308 [1]

**Details**
This change enables users to perform molecular dynamics simulations with the r2scan meta-GGA mlips coupled with the D3 dispersion correction scheme. The D3 parameters for r2scan follow the official recommendations and are compatible with the existing D3 damping framework (bj damping).

**References**
[1] Sebastian Ehlert, Uwe Huniar, Jinliang Ning, James W. Furness, Jianwei Sun, Aaron D. Kaplan, John P. Perdew, Jan Gerit Brandenburg; r2SCAN-D4: Dispersion corrected meta-generalized gradient approximation for general chemical applications. J. Chem. Phys. 14 February 2021; 154 (6): 061101. https://doi.org/10.1063/5.0041008

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7f680de2968e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.