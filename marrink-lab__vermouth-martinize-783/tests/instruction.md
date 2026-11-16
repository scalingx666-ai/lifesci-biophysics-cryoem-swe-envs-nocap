# Issue

**Title:** Add [meta] category protein to charmm lipidations

## Description

As per #782  `charmm/lipidations.rtp` lack `meta` field as in protein residues from `charmm/aminoacids.rtp`. 

This leads to `has_category(molecule.force_field.blocks.get(...), 'protein')` in `is_protein` evaluating to `False`, when running `martinize2`  on proteins with lipidations. This prevents `-ss` flag from functioning properly.

Added `[meta] category protein` to all lipidations in `charmm/lipidations.rtp`  to allow their use with `-ss` flag.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d0ec4df92602`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.