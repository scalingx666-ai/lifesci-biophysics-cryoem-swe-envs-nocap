# Issue

**Title:** Soft core potentials

## Description

# Added Reaction-Field and Ewald / PME support for Soft-Core potentials

Extended the soft-core potentials (Beutler and Gapsys) for electrostatic interactions to be able to work within the RF, Ewald and PME frameworks.

## Reaction-Field

The soft core behaviour is limited to the short range term, long range does not need correction since it works as a mean field theory where the particles interact with an implicit dielectric.

## Ewald / PME

The short range interaction accounts for soft-core behaviour. The long range term directly scales the charges with the electrostatic lambda.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b617dc4a3a04`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.