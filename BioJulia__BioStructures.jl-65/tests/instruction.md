# Issue

**Title:** Parse secondary structure annotations from mmCIF

## Description

While we can get this info from running DSSP, it seems reasonable to extract this information from the file when it is present.

One issue is that the [annotations used](https://mmcif.wwpdb.org/dictionaries/mmcif_pdbx_v50.dic/Items/_struct_conf_type.id.html) are more detailed than the [one-letter codes](https://pdb-redo.eu/dssp/about). I may not have handled this correctly, and perhaps it may be worth considering if we want to consider an alternative approach. (E.g., an `@enum`?) It might also be nice to include the numeric code, e.g., for TM6. (Presumably there will always be a switch in categorization before the next structure of the same type, so this is not as critical.)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `58456ce24654`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.