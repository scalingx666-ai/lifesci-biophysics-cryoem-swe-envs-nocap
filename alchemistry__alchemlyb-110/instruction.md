# Issue

**Title:** dhdl plot for the TI

## Description

Partially addresses #73

I have ported the dhdl plot for TI from the alchemical analysis.

Based on user experience, I have changed a few things.

- The user could now set custom label to the figure
- The x axis is elevated a bit to avoid bumping to the dhdl
- The user could now custom the color of the plot

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c3d240b4bf0d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.