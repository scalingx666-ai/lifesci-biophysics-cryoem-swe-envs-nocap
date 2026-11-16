# Issue

**Title:** Add bonded_type information in the comment of each atom in the charges section

## Description

Hi!
Thank you @jewettaij for the updates and for all the work done!

I'm making this small pull request as I think it could be beneficial for the user to have also the bonded_type string visible in the comment line of the charges section (where one has to select the proper type), as it can help to understand which bonded interactions will be selected down the line!

The only drawback I forsee is if some type will have different bonded_type for bond/angle/dihedral, but that's never the case for now, I think.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9b299530e334`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.