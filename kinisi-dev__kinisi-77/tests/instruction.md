# Issue

**Title:** 'Scipp'ification of the MDAnlysis parser

## Description

Implemented MDAnalysisparser with get_structure_coords_latt and get_indices. 

Things of note:

•Do we need to calculate volume within the parsers? - where do we then use it, I currently have not but can do if required, just an extra calc on the initial lattice. 
 
•Should  the fractional coordinates be dimensionless as discussed previously?
 
•Didn't implement the framework_indices stuff as you did not implement for the pymatgen parser, is this correct?
 
•Had to force the msd array to be float64 as MDAnalysis uses float32 by default
 
•I explicitly specified the function inputs for  _from_universe &  from_universe because something was breaking when i didn't will look into more tomorrow

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1d3221bf88c1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.