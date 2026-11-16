# Issue

**Title:** GROMACS 2025/2026 patches: tell PLUMED about checkpoints before the force calculation (alternative to #1430)

## Description

##### Description

This is an alternative to #1430 by @DrDomenicoMarson, who found and diagnosed the problem. It fixes the same bug in a smaller way, and also covers domain decomposition and the modular simulator. While reviewing the patches, I also found two small bugs, fixed in separate commits (see below).

**Checkpoints (`94a4609`).** With the GROMACS 2025 and 2026 patches, PLUMED learns that GROMACS writes a checkpoint only from the `MDModulesWriteCheckpointData` notification. In the legacy simulator, this arrives after the force calculation of the checkpointing step, and only on the main rank. Two problems follow for the actions that write their state on checkpoints. These are OPES_METAD and OPES_EXPANDED with STATE_WFILE and no STATE_WSTRIDE, METAD and PBMETAD with GRID_WSTRIDE (they write the grid every GRID_WSTRIDE steps and at each checkpoint), and others such as DRR, METAINFERENCE and the ISDB actions with a write stride:
- **On one rank:** the state is written one step after each checkpoint, and never at the final checkpoint. Restarting from the final checkpoint then fails, or uses an older state.
- **On more than one rank** (domain decomposition, also with multiple walkers): only the main rank writes, and writing a PLUMED file synchronizes all the ranks. So the run deadlocks at the first checkpoint.

The patch already calls the PLUMED force provider from the MD loop before the force calculation, on all ranks, to ask whether it needs the potential energy (`requestsPotentialEnergy(step)`). This call comes after `decideIfCheckpointingThisStep()`, so it now also passes the checkpoint decision: it is renamed `prepareStep(step, isCheckpointingStep)`, and the force provider sets `doCheckPoint` there. The flag is set to 0 or 1 on every step, because PLUMED resets it only on steps when some action is active. Unlike #1430, this needs no new notifier type in `mdmodulesnotifiers.h`, and no new subscription in the PLUMED module.

**The modular simulator** (e.g. md-vv) does not call `prepareStep()`. It writes its checkpoints before the force calculation of the step, so the late notification is on time, but it still reaches only the main rank. The main rank now records the request, and `calculateForces()` sums it over the ranks, with `gmx_sumi` in 2025 and `MpiComm::sumReduce` in 2026, before PLUMED's calculation. This only happens when `prepareStep()` is never called, so the legacy simulator has no extra communication.

As with the GROMACS 2024 patch, which also sets `doCheckPoint` before the calculation, the STATE written at a checkpoint includes that step's deposition. On restart, OPES and METAD skip the update of the first step, so a restarted run is not bitwise identical to a continuous one.

**Other fixes:**
- **`-replex` with `-plumed` (`4698ff3`, 2025 and 2026).** The bias was read into a `double`, but `getBias` writes the MD code's `real`. With GROMACS in mixed precision, PLUMED's type check stopped mdrun at the first step ("This command expects a type with size 4"), even when PLUMED computes no bias.
- **`-multidir` without `-plumed` (`dbce82c`, 2025 only).** GROMACS makes the simulations of a `-multidir` run share state, and so step in lockstep, when any module subscribes to the multi-simulation record. The 2025 module subscribed unconditionally, so `-multidir` runs without PLUMED deadlocked when the simulations had different numbers of steps. The 2026 patch already subscribes only when PLUMED is active; the same code is now in the 2025 patch.

##### Target release

I would like my code to appear in release __v2.10__

##### Type of contribution

- [x] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [ ] changes to a module not authored by you
- [ ] new module contribution or edit of a module authored by you

##### Copyright

- [x] I agree to transfer the copyright of the code I have written to the PLUMED developers or to the author of the code I am modifying.
- [ ] the module I added or modified contains a `COPYRIGHT` file with the correct license information. Code should be released under an open source license. I also used the command `cd src && ./header.sh mymodulename` in order to make sure the headers of the module are correct.

##### Tests

- [ ] I added a new regtest or modified an existing regtest to validate my changes.
- [ ] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

The CI does not build GROMACS, so I tested by hand:
- **Setup:** GROMACS 2025.0 and 2026.4, built with MPI and patched with this branch and with current v2.10, running with the PLUMED v2.10 kernel.
- **System:** ala2, OPES_METAD with `STATE_WFILE` and `STORE_STATES` but no `STATE_WSTRIDE`, and `-cpt` small enough to give 10–22 checkpoints in 20000 steps.
- **Check:** each STATE written must be at the step of a checkpoint in `md.log`.

| run | v2.10 (2025.0 and 2026.4) | this PR (2025.0 and 2026.4) |
|---|---|---|
| leap-frog, 1 rank | each STATE 1 step late, none at the last checkpoint | all match |
| leap-frog, 2 ranks (DD) | deadlock at the first checkpoint | all match |
| md-vv (modular simulator), 1 rank | all match | all match |
| md-vv, 2 ranks (DD) | deadlock at the first checkpoint | all match |
| 2 walkers (WALKERS_MPI, adaptive SIGMA) × 2 ranks | deadlock at the first checkpoint | all match |

The same runs with METAD (`GRID_WFILE`, `STORE_GRIDS` and a `GRID_WSTRIDE` that is never reached, so each grid is written at a checkpoint, plus one at step 0), leap-frog: on 1 rank, v2.10 misses the grid of the last checkpoint and this PR writes one for each checkpoint; on 2 ranks, v2.10 deadlocks at the first checkpoint and this PR writes one for each checkpoint. PBMETAD uses the same code.

Also checked:
- **Restart:** restarting from the final checkpoint and its STATE works; with v2.10 there is no STATE at the final checkpoint.
- **ENERGY:** the ENERGY CV still equals the GROMACS potential energy, on 1 and 2 ranks.
- **`-replex 50 -plumed`** (replicas at 300 and 320 K, a PLUMED input that only prints): v2.10 stops at step 0 in both versions; with this PR the exchanges run.
- **`-multidir` without `-plumed`** (simulations of 1000 and 5000 steps): with v2.10, 2025.0 hangs; with this PR it finishes in 0.2 s, as 2026.4 already did. Walkers with `-plumed` still checkpoint at the same steps.
- **Patch files:** patching a clean GROMACS tree gives exactly the files of this branch.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5b11fc997163`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.