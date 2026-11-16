# Issue

**Title:** selection syntax

## Description

@jgreener64 @timholy 

This PR is not necessarily to be merged (unless for the fact that it introduces something potentially useful and not breaking at all). 

How this syntax works may be useful for the discussions of 

I adapted the selection syntax as implemented in PDBTools for BioStructures.

With the PR, we can do (for example):

```julia
julia> pdb = read(BioStructures.TESTPDB, PDB)
ProteinStructure structure.pdb with 1 models, 5 chains (A,B,C,D,E), 19534 residues, 60570 atoms

julia> filter(sel"element H", pdb)
39817-element Vector{AbstractAtom}:
 Atom HA with serial 6, coordinates [-8.185, -15.947, -6.894]
 Atom HB1 with serial 8, coordinates [-10.428, -14.602, -7.6]
 ⋮
 Atom H1 with serial 62025, coordinates [13.218, -3.647, -34.453]
 Atom H2 with serial 62026, coordinates [12.618, -4.977, -34.303]

julia> filter(sel"chain A", pdb)
2541-element Vector{AbstractAtom}:
 Atom N with serial 1, coordinates [-9.229, -14.861, -5.481]
 Atom HT1 with serial 2, coordinates [-10.048, -15.427, -5.569]
 ⋮
 Atom H32 with serial 4011, coordinates [13.564, -16.517, 12.419]

julia> filter(sel"backbone and resname LYS", pdb)
8-element Vector{AbstractAtom}:
 Atom N with serial 307, coordinates [-8.851, 4.954, 6.211]
 Atom CA with serial 309, coordinates [-9.459, 5.331, 7.476]
 ⋮
 Atom C with serial 559, coordinates [2.983, 8.729, -0.894]
 Atom O with serial 560, coordinates [3.807, 8.269, -0.105]
```

The syntax is still limited. We do not support parenthesis, and other features that VMD syntax supports. Ideally we would be able to parse something like `(resname ARG LYS) and (not backbone) and (chain A or model 2)`. Nevertheless, is what we have in PDBTools right now and that is useful for most of our purposes (and we switch to standard julia functions for more complicated things).

This is related to the other discussion in the sense of what we want these selections to return. Now they return vectors of atoms, that is, flat structures. We could return (that would require some work) return a proper hierarchical structure, respecting the model, chain, residue, etc, of the original tree.

Food for thought. Hope this helps.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `04f6ffa2e722`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.