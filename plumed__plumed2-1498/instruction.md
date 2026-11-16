# Issue

**Title:** kenref: target GROMACS 2026 in the helper scripts, and say "patch" not "batch"

## Description

##### Description

The kenref module's helper scripts (`src/kenref/`) still targeted GROMACS 2025, and warned that PLUMED's GROMACS integration "regressed after 2025". That stopped being true once the `gromacs-2026.0` patch landed (#1439, #1456). This updates them:

- **Default GROMACS:** the fetch now takes the newest v2026.x (v2026.4 today) instead of the newest v2025.x. Any `latest-YYYY` works, and an explicit tag is used as given.
- **Patch engine:** the automatic choice now matches the GROMACS year of the source. Before, it always took the newest 2025 patch, even for a 2026 tree. If this PLUMED has no patch for that year, the script stops and asks for `--patch-engine`.
- **Version check:** 2025.x is still accepted, with a warning recommending 2026.x (pref. 2026.4). Versions before 2025 are still refused.
- **"batch" → "patch":** "batch" was a misspelling of the `plumed patch` step. `build-and-batch.sh` is renamed `build-and-patch.sh`, and the wording changes in the messages and in `install.md`. The old name stays for one release as a one-line wrapper, because KEnRef's own build calls the script by that name. KEnRef already prefers the new name and falls back to the old one; the wrapper can go in the next release.

Tested end to end on Linux (OpenMPI 5 + clang 20, libc++). From an empty state, `build-and-patch.sh -y` built KEnRef and PLUMED, fetched GROMACS v2026.4, patched it with `gromacs-2026.0` (no rejected hunks), and built and installed it. A short `gmx_mpi mdrun -plumed` run of the KEnRef SIGMA test system with the patched GROMACS gives the step-0 energy expected from KEnRef's own GROMACS interface (3.65155e-05). The same run without `WHOLEMOLECULES` is refused as torn, as it should be, and a two-replica `-multidir` run completes. The tag lookup and the year-matched patch choice were also checked for 2024, 2025, 2026 and a year with no patch.

##### Target release

I would like my code to appear in release __master__

##### Type of contribution

- [ ] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [ ] changes to a module not authored by you
- [X] new module contribution or edit of a module authored by you

##### Copyright

- [X] I agree to transfer the copyright of the code I have written to the PLUMED developers or to the author of the code I am modifying.

##### Tests

- [ ] I added a new regtest or modified an existing regtest to validate my changes.
- [ ] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

These are build helper scripts that no regtest runs; they were tested by the end-to-end build described above.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e5a05b184652`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.