# Issue

**Title:** Keep waters that carry an alternate-location label

## Description

.

Waters in a chain share `label_seq_id` `.`. `PDBxFile` used `(label_seq_id, chain, atom name)` as the atom key, so every water oxygen looked like the same atom. A later water with `label_alt_id` `A` was treated as a second conformer and dropped, including waters that only have one position.

The key now includes the author residue number and insertion code. `struct_conn` still looks up atoms by the old three-field key, because those records do not carry `auth_seq_id`.

Test is the glycine + HOH 101 / 102 / 103 file from the issue. Dual-alt 102 keeps the first location.

I used Cursor to help write the patch. I checked the key collision against the issue reproducer and ran the new test on master (fails before, passes after).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `146a320f97bd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.