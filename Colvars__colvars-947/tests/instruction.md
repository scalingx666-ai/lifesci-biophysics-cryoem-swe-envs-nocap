# Issue

**Title:** Check that pull requests can be cherry-picked onto branches

## Description

Ensure that bugfixes can be easily backported onto a list of pre-defined maintenance branches.  Currently, this list includes:
- `gromacs-2026`
- `namd-3.0`
On those branches, only PRs labeled `bugfix` are tested

Also tests other development branches:
- `gromacs-2027`
- `lammps-develop`
- `namd-3.1`
because they may become maintenance ones later on.  We probably have to agree on how to update them (fast-forwarding everything from `master` on a rolling basis, or do that only when the PRs to those packages are merged).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e2eaf0a2d250`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.