# Issue

**Title:** Fix stochastic failures in RepartitionedHybridTopologyFactory test

## Description

## Description

<!-- Describe your changes in detail. -->
Changes  to `test_relative.py::test_RepartitionedHybridTopologyFactory_energies`:
- Only test ALA -> SER (not ALA -> all other amino acids)
- Only propose geometry once (not 5x)

Changes to `RepartitionedHybridTopologyFactory`:
- Fix bug in instantiation of `NonbondedForce`

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->
We were previously seeing occasional failures with `test_relative.py::test_RepartitionedHybridTopologyFactory_energies`. This was due to the geometry engine proposing clashy positions, which have very, very large energy, causing the `validate_endstate_energies()` check to fail because the energy threshold of 1e-4 becomes difficult to pass with very large energies.

The changes introduced here stop testing ALA-> all other amino acids and instead restricts testing to ALA -> SER mutations, because its unlikely that the geometry engine will propose clashy positions for this conservative mutation.

I also caught a bug in the `RepartitionedHybridTopologyFactory` class implementation where the a `NonbondedForce` was not instantiated properly.

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->
See `test_relative.py::test_RepartitionedHybridTopologyFactory_energies`

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dbe616b04f44`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.