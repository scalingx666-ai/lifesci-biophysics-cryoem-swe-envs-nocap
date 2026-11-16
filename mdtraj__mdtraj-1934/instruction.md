# Issue

**Title:** Fix PDB overflow (v2) + Allow PDB to guess atom/residue numbers

## Description

A continuation of the discussion on #1931. Currently mdtraj errors out with ugly ValueErrors if it encounters non-integer residue number/atom number.

Incidentally, if you did not use your pdb as a topology file (i.e. no PdbStructure), you might end up with all 0s as residue numbers (but will load successfully)!

So to deal with the inconsistencies, it will now "guess" the residue numbers and atoms if it overflows in a non-standard way, doesn't exist or if it's rubbish (a str). The residue number and atom numbers will be the previous number + 1.

----

@mattwthompson While looking at the code, there were some inconsistencies in the treatment of residue/atom number errors, so I made it so it'll just guess all the residue/atom numbers (basically lists them sequentially) if it doesn't understand what is passed in the atom/residue number columns. I understand that most residue info could be rubbish, but for atom/residue numbers we can at least do a guess (and maybe raise a warning that we guessed)? If this is not a preferable behavior, I think we could at least throw a more informative error ('Your pdb file is malformed/missing information!') before crashing than just a ValueError with an int-conversion failure.



EDIT:
Now also  with additional test cases.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bf857af4aaf9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.