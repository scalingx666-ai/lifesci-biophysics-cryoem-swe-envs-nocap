# Issue

**Title:** Alternate/optional fields for CIF parsing

## Description

Some modeling tools (e.g., Boltz-2) write CIF files that we can't
currently parse. The CIF standard lists `_atom_site.label_atom_id` as a
required field, but we use `_atom_site.auth_atom_id`. This allows both.
It also makes `_atom_site.pdbx_formal_charge` optional.

Reference:
https://mmcif.wwpdb.org/dictionaries/mmcif_pdbx_v50.dic/Categories/atom_site.html#

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c70cc28913d9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.