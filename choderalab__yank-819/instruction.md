# Issue

**Title:** Bugfix and add replica state index statistical inefficiency to analysis

## Description

In this PR:
* Fix #816: Setup logs are saved in the wrong folder.
* Fix #818: crash with single FlatBottom/Harmonic restrained atom.
  - I've slightly refactored the descriptor `_RestrainedAtomsProperty` to remove shared code between Boresch-style and radially symmetric restraints and to always casts integers to a list so that we don't need to worry about this extra case in the internal logic.
* Fix #815: Better way to generate residue name for SMILES and sdf molecules.
  - We now generate a residue name in the form `YXX` where `Y` is an uppercase letter and `XX` are two digits. This gives us a total of 2600 unique residue names. The algorithm fails to maintain unique residue names if the list of molecules is updated in the middle of the setup, but I couldn't find a better solution to keep supporting parallel setup. Suggestions are welcomed. We never use the `SetupPipeline` class this way anyway, so this should not be a problem.
* Fix #817: Compute statistical inefficiency of replica state index in analysis.
  - I've added this to both the text analysis and the notebook.
* I've also fixed the alignment of the final printed free energy. I broke it several months ago and it was driving me crazy.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0b8d725e4713`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.