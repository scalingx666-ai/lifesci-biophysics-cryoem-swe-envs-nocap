# Issue

**Title:** Progress indicators for path density; move summary

## Description

This adds progress indicators (i.e., tqdm support; introduced in #882) for path density/path histogram objects, as well as for the `move_summary`. With tqdm installed, you automatically get progress bars for these analyses.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `79ca42f04cb1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.