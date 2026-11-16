# Issue

**Title:** Remove legacy code related to chiral conversion

## Description

Completes the removal of code that was added in #1356 and #1360  to work around our initial lack of support for chiral volume conversions (since added in #1406).

Highlights:
- Completely removes the atom mapping leaf filter `enforce_chirally_valid_dummy_groups`. (The infrastructure supporting arbitrary leaf filters is left in place for now.)
- Simplifies dummy group selection to choose an arbitrary assignment independently for A and B (since we no longer need to jointly select A and B to avoid chiral volume conversions)
- Re-unifies end state setup code (since we no longer have a use case for parameterizing bonds and chiral volume restraints separately)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4657b53cc2da`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.