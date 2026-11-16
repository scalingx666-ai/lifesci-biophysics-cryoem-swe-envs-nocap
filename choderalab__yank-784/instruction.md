# Issue

**Title:** Add solute-only trajectory storage

## Description

This PR adds the ability for YANK to store only the solute trajectory as part of the analysis file. Minimal changes to the API calls in the form of new keywords; no new public methods nor changes to existing calls which would break backwards compatibility.

This is constructed automatically from the Topography if the appropriate flag is set in the YAML file, which defaults to True.

There are no new tests for this at the moment, which we may want to write before merging, but I at least want to get the conversation started and the code reviewed.

This has also changed the logic in the trajectory analysis code, so that will need to be verified (again, tests would fix)

Progress towards #777

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e7bfb796f1b5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.