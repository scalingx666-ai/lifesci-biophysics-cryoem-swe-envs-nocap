# Issue

**Title:** Revival of `nucleic` and `is_nucleic` selection PR

## Description

Revival of #1398. I've made it compatible with the current main branch and added `nucleic`/`is_nucleic` to docs.

@dwhswenson - are you ready for this forever PR to be merged? I think it would be a nice addition.

As far as review goes, I've reviewed it and tested it with some PDBs and with an Amber trajectory (in addition to the provided tests) and all seems to work as intended. I'd be in favor of leaving the Amber residue names in as default.

I'd approve it if I could, but having opened this PR, I can't self approve :)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cf11428b49c2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.