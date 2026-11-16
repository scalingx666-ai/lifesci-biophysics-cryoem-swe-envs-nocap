# Issue

**Title:** Addressing some issues with `save_cif`

## Description

I recently encountered an error when using `traj[frame].save_cif()`. Using `save_pdb()` there is no problem, but during the header/bond info writing of the `cif` file, I came across `key_errors` for chain and residue IDs.

Tested with `mdtraj-1.11.0`.

My code to reproduce this currently looks something like the following:
``` Python
import mdtraj as md
traj = md.load_dcd("traj.dcd", top="traj.pdb")
traj[0].save_cif("frame.cif")
```

The issue seems to stem from the following dicts: https://github.com/mdtraj/mdtraj/blob/9ca89d82caf6a89d96ca464dba1ca394c041de60/mdtraj/formats/pdbx/pdbxfile.py#L489-L490

When these are indexed later on:
https://github.com/mdtraj/mdtraj/blob/9ca89d82caf6a89d96ca464dba1ca394c041de60/mdtraj/formats/pdbx/pdbxfile.py#L507-L520

I am getting a `key_error` since, e.g. `atom1.residue.chain` is a mdtraj chain object, but for some reason the memory address does not match with either of the chain objects in `chainIds`. A similar issue happens when indexing `resIds`. 

This initial issue lead me to update the following:
* I removed the header arg from the `save_cif` trajectory method, this was not being used anyway and there needs to always be a header to loop the atomic coordinate data being stored.
* In the same `save_cif` method I also exposed and propagated the `keepIds` arg through to the write methods, allowing for user specification.
* With the `keepIds` arg, I noted that when setting to `True`, there was attribute errors when trying to access the topology chain and residue IDs, I updated these attribute calls to what I think are the corrected ones.
    * I also updated the `insertionCode` checking to account for a missing `insertionCode` attribute.
* I then addressed my original `key_error` issue by accessing the chain and res IDs directly from the `atom1` and `atom2` objects, instead of indexing the `chainIds` and `resIds` dicts. This fixes my issue but at cost; now the `keepIds` functionality is not being used in the bond writing part of the header writing in `cif` file saving.

To truly address the issue, some more investigation is needed. Perhaps the `bond.atom.residue.chain` or the topology-based chain objects are getting overridden somewhere? I'm also curious why this issue hasn't come up before, I'm loading in an `OpenMM` generated `dcd` traj file and a `pdb` file for the topology. Perhaps there is something with this specific combination?

Apologies for the unfinished PR, I thought it would be an easy fix but got a bit stuck. If anyone has any thoughts or suggestions here I would greatly appreciate any help.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a0aa1024adba`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.