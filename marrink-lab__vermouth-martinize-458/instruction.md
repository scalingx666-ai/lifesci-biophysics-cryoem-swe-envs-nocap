# Issue

**Title:** fix modifications

## Description

This fixes issue #384. However, there is one caveat: Now the modification can have an order attribute. This originates from the way we treat the atoms when making interactions. Even though we maybe don't want this, having order attributes is most consistent in my opinion with the fact that modifications is a subclass of `Link`. I would propose to later down the line disentangle the Modifications object from the Link, because they are really very separate things. As far as I can asses it makes no difference between having order attributes as to not having them in the current code.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `aa53b3e3ca73`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.