# Issue

**Title:** Change Boresch Restraints energy function and auto-atom method

## Description

The Boresch restraints now pick their atoms based on bonded atoms only
to reduce the chance the two outer torsions can drift to co-linear,
causing NaN's. I tied to pick make the algorithm not include new
dependencies like a graph module (since its so simple), but its a bit
clunky right now and could use a look over if you could, @andrrizzi

The energy function of the torsion restraint in the Boresch is now

```
(K/2) * (1-cos(phi - phi0)) * (pi^2)/2
```
which is an always positive function wrapping at `phi-phi0 = [0, 2*pi]`.
The `pi^2` scaling factor ensures the function has the same maximum as the
`(phi-phi0)^2` harmonic function, but does have a steeper slope overall.
I'm open to discussion on this.

Because the functional form changed, there is no longer an analytical
solution to the standard state correction (its all numeric),
everything related to the `standard_state_correction_method` has been removed

cc @jchodera for discussion on the math parts and energy function choices.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a5b033895c36`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.