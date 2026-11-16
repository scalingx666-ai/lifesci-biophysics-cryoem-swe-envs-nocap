# Issue

**Title:** Fix naked charge padding

## Description

## Description

Edit the "naked charge fix" introduced by #840 by making sure hydroxl hydrogens in protein.ff14SB are handled properly (sigmas for hydroxyl hydrogens are changed from 1.0 nm to 0.06 nm).

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->
I had previously introduced a "naked charge fix" in #840, where I add a small padding if an atom's sigma/epsilon are 0. If epsilon is 0, we should set it to 0.0001 kJ/mol and if sigma is 0, I set it to 0.6 angstroms.

However, for hydroxl hydrogens in `protein.ff14SB`, the LJ parameters are defined as:
```
<Atom type="protein-HO" sigma="1.0" epsilon="0.0"/>
```
With my previous "naked charge fix", a padding would be added to epsilon, but sigma would remain 1 nm, which is much too large. 

In this PR, I change sigma to 0.06 nm if the particle's original epsilon was 0 kJ/mol and its sigma was 1 nm. I also add logger statements to alert the user when the sigma/epsilon is being changed.

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `58bd6e626e02`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.