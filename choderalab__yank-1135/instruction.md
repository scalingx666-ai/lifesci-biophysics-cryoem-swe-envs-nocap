# Issue

**Title:** Add command-line option for writing status file during simulation

## Description

Implements #1131 

This PR adds a `--status` optional argument to the `yank script` CLI mode that writes a `status.pkl` file to the experiment directory at the end of each experiment switch interval. This is a stopgap feature that will allow us to assess the progress of calculations despite the NetCDF issues we are encountering.

I've added a test that runs a short cyclodextrin host:guest calculation in vacuum to make sure the `status.pkl` file is emitted.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bbba6b996592`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.