# Issue

**Title:** reduce_hydrogen: do not strip H whose parent atoms are already bonded

## Description

`workarounds_00345` rule 003 removes a pair of H closer than 1.0 A when their
parent heavy atoms are within 1.6 A, on the premise that the parents carry a bond
the restraints did not perceive. A bond that was perceived satisfies the same test.

Methyls are placed at an arbitrary torsion and rotated to staggered later, so an
as-placed methyl can sit eclipsed against its own parent's H. On 4RXN, THR 5's
CB-CG2 is an ordinary 1.52 A bond with HG22 0.84 A from HB, and both atoms are
deleted one step before the rotation that would have separated them. THR 7 and
THR 28 in the same file keep theirs only because their starting torsion landed
further away.

This skips the pair when the two parents are already bonded, using the
connectivity the function builds two lines above. The four ligand cases in
`tst_add_hydrogen_12` are unaffected: their links really are unperceived, so the
parents are not bonded and the rule still fires. That test still fails on its
first case if the workaround is disabled outright.

`n_H_final` was also read before the workaround ran, so the reported count never
saw the deletion: 4RXN prints 372 hydrogens added and writes 370. Moving the
assignment below the call fixes it.

`tst_add_hydrogen_14` covers both, on a 25-atom cut of 4RXN. The count assertion
is a guard against a future deletion added after the count is taken; it cannot
fail with the rule 003 fix in place.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `53becfb2be4a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.