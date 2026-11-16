# Issue

**Title:** Fix bug in bias weight random noise.

## Description

The random noise in any biases weight used a spread of `1-weight`. When the deterministic (measured) part of the bias scoring has very small values (TargetCoordinate approaching the target), this noise spread was dominating the deterministic part of the score, even at relatively high weights (0.5-0.8ish). This is a quick fix that scales the spread used in the noise part of the score by the actual spread present in the measured values (distances, density, etc..) so that it remains consistent regardless of units/magnitude.

### PR Summary:

### PR Checklist
------------
 - [x] Includes appropriate unit test(s)
 - [x] Appropriate docstring(s) are added/updated
 - [x] Code is (approximately) PEP8 compliant
 - [x] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e8ded6f7251c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.