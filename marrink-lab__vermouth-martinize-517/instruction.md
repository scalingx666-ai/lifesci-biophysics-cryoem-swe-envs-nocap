# Issue

**Title:** [VIPR] Parser for [ meta ] information

## Description

This PR contains my proposition for parsing block meta. 
I suggest keeping the format separated by tab or space, the first element will be the key and the rest will be the value(s). Then it allowed the user to add extra information to .itp file, and retrieve it through a dictionary. 

Example : 

        [ moleculetype ]
          GLY  3
        [ atoms ]
         1 P4 1 ALA BB 1
     
        [ meta ]
        group protein polymer organic; test
        realname Glycine


Output : 
( ff.blocks['GLY'].meta )
     {'group': ['protein', 'polymer', 'organic'], 'realname': 'Glycine'}

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1fba29ead1d7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.