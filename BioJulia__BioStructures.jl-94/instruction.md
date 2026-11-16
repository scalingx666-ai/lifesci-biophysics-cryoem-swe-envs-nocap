# Issue

**Title:** Add modified amino acid residues to `amino_acid_data`

## Description

Heya,

This PR adds common modified amino acid codes to `amino_acid_data` so that these residues are no longer dropped in `proteinselector` calls. I've currently chosen to add the actual name of the modified residue but still use the original three letter codes so that the modified residue references its parent aa. Also, I've set the one letter code to be the lowercase of the original letter so that modified residues are identifiable by a lowercase letter.

I had Claude fill in the remaining amino acid properties (mass, charge, etc.), so these should be double checked.

Let me know if you'd like to see changes to the letter codes!

Alex

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e405cb7d6c01`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.