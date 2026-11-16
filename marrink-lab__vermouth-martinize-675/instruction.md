# Issue

**Title:** Protect angles in helices when adjacent to prolines (2nd try)

## Description

Re-submitting this PR because its original incarnation (#673) was mistakenly closed too early. See the discussion there.

The current implementation weakens helical BB-BB-BB angles whenever a Pro is present, which makes sense.

The helical dihedrals, however, are never switched off (shouldn't they be?). Helix BB-BB-BB angles centered on Pro are, therefore, protected by type 10 angles from reaching 180 deg.

Angles involving adjacent residues, however, are problematic because they are also weakened, but not protected by type 10 potentials. This PR fixes that.

.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c3f654deeba4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.