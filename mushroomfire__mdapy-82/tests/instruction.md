# Issue

**Title:** md_elastic_constants: a Langevin thermostat in the strained runs, symmetric constants

## Description

A bug fix found by the comparison with the stress-fluctuation method.

**The bug.** After the strain, `md_elastic_constants` kept each strained crystal at the temperature with a Nosé–Hoover thermostat (`fix nvt`). A deterministic thermostat does not bring all the vibrations of a weakly anharmonic crystal back to the temperature after a sudden strain. For silicon with the Tersoff potential (216 atoms, 300 K), C11 and C12 came out about 2 GPa (2 %) too high, 111.4–112.4 and 77.9–78.1 over four seeds. One run's blocks scattered by 1.8 GPa, so the quoted errors (0.85–1.26) were unreliable.

**The fix.**
- **Thermostat.** The strained runs use `fix langevin` with `fix nve`. Both signs of a component share the seed, so the difference of their stresses cancels part of the noise. The damping is 100 time steps, as the thermostat before.
- **Symmetry.** Each entry off the diagonal is now the mean of its two estimates, C_jk and C_kj, from independent runs, with their errors combined. Before, the two differed by about 0.5 GPa rms.
- **Docs.** The page describes the thermostat, the symmetry, and the 1 % strain at low temperature: +0.2 % on C11 of copper at 20 K, where 0.2 % with longer runs gives the small-strain limit.

**Results** (Triton, Xeon Gold 6248). The fixed function, defaults with timestep 0.002/0.001 ps, 4 seeds each at 300 K:

| | C11 | C12 | C44 |
|---|---|---|---|
| Si, Tersoff, 216 atoms | 109.29–109.53 (± 0.50–0.63) | 75.49–75.91 | 9.71–9.94 |
| Si, stress fluctuations | 109.53 ± 0.07 | 75.80 | |
| Cu, Mishin EAM, 864 atoms | 163.79–164.03 (± 0.31–0.43) | 119.89–120.03 | 71.69–71.76 |
| Cu, stress fluctuations | 163.81 ± 0.04 | 119.87 | 71.69 |

The same check with the strain method in a study script: Langevin gave 109.66 ± 0.20 / 76.00 ± 0.22 for silicon, and Nosé–Hoover 110.3 ± 1.8 / 77.0 ± 1.7.

**Tests.** The constants are symmetric; the same seed gives the same constants and another seed others. The cold crystal still equals the static constants, a sheet still equals its static constants, and the hot crystal is softer.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6d81f37aa9ab`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.