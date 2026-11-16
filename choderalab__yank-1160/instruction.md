# Issue

**Title:** Truncated octahedral box geometry in Leap class

## Description

Hi!

Leap class in utils.py can be very useful, even out of yank. Would you support the possibility to produce truncated octahedral boxes with it?

The small modification necessary in Leap.solvate() is included in this PR with a new input argument ` box_geometry` taking the values "cubic" (default) or "truncated_octahedral".

Thanks in advance.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4dfcc8e127c5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.