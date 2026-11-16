# Issue

**Title:** Add bond metadata (order and type) and formal charge to HDF5

## Description

This PR addresses #1930 - it adds **bond order**, **bond type**, and **atom formal charge** to the topology data stored in HDF5. The goal is to enable full round-tripping of a topology: if a user saves a topology to HDF5 and loads it again, they should get a topology with the same information. The current implementation of the HDF5 does not allow this, particulary for bond information.

Currently, the HDF5 format only stores which atoms are bonded (pairs of indices) and does not store the new atom formal charge field. As a result, a user might save an MDTraj trajectory as HDF5, reload it, and get a different topology object (bond order and bond type are lost on write). This PR fixes that by adding a new HDF5 field, `bond_metadata`, which stores metadata for each bond in the same order as the `bonds` list. This keeps the format more backward compatible by keeping the existing `bonds` representation.

For atoms, this PR adds an optional `formal_charge` field. If at least one atom has a formal charge, the field is written; if all formal charges are `None`, it is omitted. This avoids having adding `formal_charge: None` unneccessarily to all files.

When writing HDF5 files, these additions are enabled with the keyword argument `bond_metadata=True` to the `HDF5File` class. When using top level `save` or `load` functions, these fields will now be written by default. Note that the `bond_metadata` field is a special field for the HDF5 file format. Its information is loaded into bond objects on the MDTraj trajectory - it's just use for reconstruction.

**Resulting JSON structure (simplified):**

```
{
  "chains": [
    {
      "residues": [
        {
          "atoms": [
            {"index": …, "name": …, "element": …, **"formal_charge": …**},
            …
          ]
        },
        …
      ]
    },
    …
  ],

  "bonds": [
    [i0, j0],
    [i1, j1],
    …
  ],

  "bond_metadata": [
    {"order": …, "type": …},
    {"order": …, "type": …},
    …
  ]
}
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e98f7aa18f02`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.