# Issue

**Title:** propagate edge attributes in res-graph making

## Description

this is the left-over of #334. This PR makes sure the res-graph making propagates edge-attributes IF they are somehow given. For polyply I now subclass the ff-parser, which seems the only way to make this work.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a3ad7e14fc0c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.