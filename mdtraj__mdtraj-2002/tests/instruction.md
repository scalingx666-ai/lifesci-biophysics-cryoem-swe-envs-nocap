# Issue

**Title:** Allow ArcTrajectoryFile to optionally take topology

## Description

Been recently working on `.arc` trajectory files (yay Tinker). While the file itself contains _some_ information on topology, it does not have residue information. This means all atoms are [funneled into a single residue](https://github.com/mdtraj/mdtraj/blob/main/mdtraj/formats/arc.py#L100-L103). ([actual code](https://github.com/mdtraj/mdtraj/blob/main/mdtraj/formats/arc.py#L353-L356))

This PR adds in the functionality to feed in a topology (file name, trajectory objects, or topology objects) to `load_arc()`, allowing users to better read in arc trajectory files for analysis. Most of the code is essentially modeled off of the [NetCDF trajectory loader](https://github.com/mdtraj/mdtraj/blob/main/mdtraj/formats/netcdf.py), which requires a topology. Existing behavior for not feeding in a topology is retained.


Testing script:
```
import mdtraj
a = mdtraj.load('seg.arc', top='seg.pdb')
b = mdtraj.load('seg.arc')

print(len(a.topology._residues))  # prints 2112
print(len(b.topology._residues))  # prints 1

print(a.topology._residues[0])  # prints TYR1
print(b.topology._residues[0])  # prints RES1
```

Remove the `.txt` to run. Wouldn't upload to GitHub otherwise.
[seg.arc.txt](https://github.com/user-attachments/files/19250715/seg.arc.txt)
[seg.pdb.txt](https://github.com/user-attachments/files/19250717/seg.pdb.txt)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2effa667c7d2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.