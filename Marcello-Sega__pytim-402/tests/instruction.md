# Issue

**Title:** Contact angle observable reimplemented

## Description

* implementation of instantaneous contact angles and other geometrical parameters
* implementation of ellipsoidal linear and nonlinear fitting, with resolution of contact line location and contact angle function
* new linear least squares and nonlinear fitting of ellipse, with RMSD tested to be lower than the fit to a circumference
* ellipticity constraint imposed for linear and nonlinear fit
* new procedure to remove automatically the liquid/solid interface atoms and obtain the liquid/vapur interface
* more consistent user interface

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e30e36dc0ae1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.