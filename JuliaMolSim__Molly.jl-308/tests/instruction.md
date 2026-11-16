# Issue

**Title:** Add solute dielectric to the reaction field interactions

## Description

Currently, the reaction field interactions hard-code $\varepsilon_r$ to 1:

$k_{rf} = (\varepsilon_{rf} − 1)/(2\varepsilon_{rf} + 1)/r_c^3$.

This seems to be the convention of OpenMM, on which I assume this is based. Other simulation engines such as GROMACS [allow non-unit solute dielectrics](https://manual.gromacs.org/current/reference-manual/functions/nonbonded-interactions.html#coulomb-interaction-with-reaction-field):

$k_{rf} = (\varepsilon_{rf} − \varepsilon_r)/(2\varepsilon_{rf} + \varepsilon_r)/r_c^3$ and $c_{rf} = 1/r_c + k_{rf}\cdot r_c^2$.

This PR adds a solute_dielectric keyword ($\varepsilon_r$, corresponds to GROMACS epsilon-r) to the reaction field interactions. In a second PR I would like to also add an optional reaction-field correction (see link above) for excluded atom pairs as well if you think it would be a useful addition.

The affected interactions are:
- CoulombReactionField
- CoulombReactionFieldScaled
- CoulombSoftCoreBeutlerReactionField
- CoulombSoftCoreGapsysReactionField
- SetupCoulombReactionField

The forces and potentials of the modified interactions are validated against a GROMACS 2026.0 toy system in a new test set. The current default behavior should be unchanged apart from the constructor changes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f4d31697b3aa`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.