# Issue

**Title:** treatment of MASTER lines

## Description

Hi,

Sorry for not raising an issue for this, I thought going straight to a PR, in case you agree, it can be merged directly without further discussion.

I wanted to get the backbone of a specific PDB file.

```
pdb_fetch 5d8v | pdb_keepcoord | pdb_selatom -CA,C,N,O | pdb_delresname -HOH | pdb_selaltloc -A | pdb_reatom -1 | pdb_tidy > 5d8vBB.pdb
```

This gives a very clean file except for the `MASTER` line at the end:

```
ATOM    331  C   GLY A  83       5.965  20.737   8.504  0.55  4.29           C  
ATOM    332  O   GLY A  83       7.215  20.811   8.519  0.55  4.32           O  
TER     333      GLY A  83                                                      
MASTER      378    0    9    5    3    0   29    6  847    1   76    7          
END   
```                      

Yet, I believe after so many modifications, the `MASTER` line is a bit meaningless there. I thought the same way `pdb_tidy` is removing `CONECT` lines, as discussed in #72, it maybe should remove `MASTER` as well.

What do you think? If you agree, this PR already provides the modifications and some tests.

Also, I added the `MASTER` to the docstring of `pdb_keepcoords`, because it was keeping the `MASTER` without telling that to the user.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `30a7a7acd429`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.