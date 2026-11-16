# Issue

**Title:** GROMACS 2025/2026 patches: follow-up of #1506 (constraint virial with MTS, shells in reruns and with ENERGY, rerun stop, docs)

## Description

##### Description

, which was merged while I was still running a last review. That review found a few more problems in the GROMACS 2025/2026 patches, and some details of the docs to correct. The fixes for the GROMACS 2020-2024 patches are in the follow-up for v2.9 (#1508); both can reach master with the merges of v2.10, as #1505 and #1506 did.

**Constraint virial with multiple time stepping**

When PLUMED needs the energy, it applies its forces after the force calculation; with MTS, to the combined buffer, which the update uses. On the slow steps that compute the virial, GROMACS computes the constraint virial from the normal buffer, and writes that one to the trajectory. So the constraint virial lacked the part of PLUMED's forces that the constraints pass on. The difference PLUMED made to the combined buffer is now added to the normal one too.
- Restraint between two water oxygens, MTS (factor 2), ENERGY printed every 2 steps, against the GROMACS pull code: virial off by 2.7, -0.5 and 3.8 kJ/mol at step 0; now 0, as without MTS, with the same trajectory forces.

**Shells**
- Reruns: they did not call `prepareStep()`, so with shells PLUMED still updated on each iteration of the shell relaxation, as step 1. Reruns now call it. A call is "repeated" when PLUMED already calculated since the last `prepareStep()`, rather than when it has the same step number: the frames of a rerun can share one, which is why this was limited to MD.
  - A rerun of one SW water dimer frame with unrelaxed shells printed 4 COLVAR lines, 3 of them at step 1; now 1.
- With ENERGY: PLUMED updates (prints, deposits its bias, checks COMMITTOR) on the first force calculation of the step, before the shells are relaxed. The energy it uses for that is the one before the relaxation. Making PLUMED update after the relaxation would need a new hook in `md.cpp`, and shells are rarely used with PLUMED, so mdrun now prints a warning (once) when PLUMED needs the energy with shells. The docs say so too.

**Reruns stop at the frame that asks**

A stop request (e.g. COMMITTOR) in a rerun was acted on at the start of the next frame, which was then fully processed. Now the rerun stops after the frame on which PLUMED asks, as with the GROMACS 2020-2024 patches.

**Docs**
- The table of what the GROMACS interfaces support now has notes:
  - GROMACS 2026.0-2026.3 without patches stop at startup with more than one MPI rank (fixed in 2026.4; the 2026 patch already fixes it for those releases);
  - ENERGY and the checkpoint data of PLUMED need the legacy simulator with md-vv (`GMX_DISABLE_MODULAR_SIMULATOR=1`);
  - with MTS, the energy is available only on the slow steps;
  - the limitation of shells with ENERGY.
- The multiple-walkers row is about simulations that communicate through MPI (e.g. `WALKERS_MPI`). Walkers that share files also work without patches.
- CHANGES and patch descriptions:
  - after a stop request, mdrun runs one more step after the next communication of signals, and between simulations signals are communicated every 200 steps or more;
  - with shells, PLUMED updates once per step, but calculates on each iteration;
  - `\ref` links instead of the markdown links of the master manual.

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

With the regression tests of plumed-testcenter/plumed-testcenter#95, updated for these cases (the virial at step 0 within 0.1 kJ/mol and the forces with MTS; a rerun of unrelaxed shells; the warning; the rerun stopping at the frame that commits):
- GROMACS 2025.0 and 2026.4 patched with this branch, with its kernel: all tests pass.
- With v2.10 as it is now, GROMACS 2026.4 fails `pull-mts`, `shells` and `rerun`.
- Patching a clean GROMACS tree gives exactly the files of this branch.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4b8956204984`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.