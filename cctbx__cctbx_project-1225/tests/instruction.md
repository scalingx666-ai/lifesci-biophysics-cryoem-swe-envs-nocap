# Issue

**Title:** barbed_wire_analysis: give each do_probe call its own temp directory

## Description

`do_probe` wrote its scratch PDB to a fixed, relative filename, so two analyses
running at once in one process shared it.

Two threads on AF_json_v3 and pae_model, which alone give 8984 and 6756 contact
pairs:

    trial 1   AF_json_v3 -> FileNotFoundError   pae_model -> 8687
    trial 2   AF_json_v3 -> 6756                pae_model -> crash
    trial 3   AF_json_v3 -> FileNotFoundError   pae_model -> 6756

Only the first of those raises. In trial 2 one analysis was scored on the other
model's contacts, and 8687 is a half-written file.

This uses mkdtemp per call, removed in a finally, so a failure no longer leaves
the file behind either. Output is unchanged: per-residue categories, text codes,
packing quality, all the outlier flags and both contact totals match a recorded
baseline across 644 residues, and six concurrent trials now give the right
answer every time.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0fa242a22339`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.