# Issue

**Title:** return partial convergence analysis if mbar fails for individual fraction

## Description

When running ABFE calculations in OpenFE, MBAR sometimes fails to produce an estimate at low fractions of uncorrelated samples (~0.1-0.4). It would be helpful to still return the convergence trace where MBAR can generate a meaningful result (e.g. >0.7) This PR modifies `get_forward_and_reverse_analysis` under `openmm_utils` so that `NaN` values are appended to the forwards/reverse estimate lists where MBAR fails and the successful estimates are still returned.

Checklist

- [x]  All new code is appropriately documented (user-facing code *must* have complete docstrings).
- [x]  Ran [pre-commit](https://pre-commit.com/): you can run pre-commit locally or comment on this PR with `pre-commit.ci autofix`.

## Developers certificate of origin

- [x]  I certify that this contribution is covered by the MIT License [here](https://github.com/OpenFreeEnergy/openfe/blob/main/LICENSE) and the **Developer Certificate of Origin** at https://developercertificate.org/.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `40d2e345cbdc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.