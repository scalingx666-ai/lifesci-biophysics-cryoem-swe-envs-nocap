# Issue

**Title:** Add FCHK connectivity (bonds) parsing support

## Description

"Approach"

The FCHK format stores bond info in four fields: MxBond, NBond, IBond, and RBond. I followed the approach from OpenBabel's implementation (https://github.com/openbabel/openbabel/blob/889c350feb179b43aa43985799910149d4eaa2bc/src/formats/fchkformat.cpp#L651-L690) and adapted it for IOData.

The format stores each bond from both atoms' perspectives (so an O-H bond appears twice — once for O, once for H). To get a clean bond list, I only keep the entry where atom_i < atom_j. This is the same approach OpenBabel uses.


"Tests"

I used files already in the repo that have connectivity data: h2o_sto3g.fchk, peroxide_opt.fchk, li2_g09_nbasis_indep.fchk, and water_atcharges.fchk (which has the fields but no actual bonds). Also added roundtrip tests to make sure dump and load preserve everything correctly. All 6 new tests pass.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f932fcd21277`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.