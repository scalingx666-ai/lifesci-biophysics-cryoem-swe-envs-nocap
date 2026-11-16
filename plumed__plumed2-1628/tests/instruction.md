# Issue

**Title:** Onebead mapping shared

## Description

# ONEBEAD mapping and LCPO as reusable classes (2026-11-12)

Step 1 of the plan "make the SAXS coarse-grained mapping reusable" (the deferred option 2 of saxs-split.md). Steps: (1) refactor inside isdb [done], (2) SASA_LCPO uses the shared LCPO, (3) decide on a mapping action (beads as virtual atoms).

## Branch
`onebead-mapping-shared`, 3 commits on master eb9ca9ef3, local only (not pushed).
- 74fbb269c isdb: move the ONEBEAD mapping out of SAXS into OnebeadMapping
- dd9e1115a isdb: move the LCPO surface areas out of SAXS into a class
- b561b5d46 CHANGES: OnebeadMapping and LCPO classes (developer section)

Mac: `Claude outputs/onebead-mapping/onebead-mapping-shared.bundle` (verified on the Mac). `git fetch "Claude outputs/onebead-mapping/onebead-mapping-shared.bundle" onebead-mapping-shared:onebead-mapping-shared`. Also `glycan-test-template.pdb` there: protein + RNA of rt-saxs-2, ASN19 renamed NLN and linked to a NAG-NAG-BMA-MAN chain, free FUC/GAL/GLC(+N-acetyl)/AMAN, NA and CL ions (generated geometry, for mapping tests only).

## What changed
- `src/isdb/OnebeadMapping.h/.cpp`
  - `OnebeadTypes`: bead-type enum (TRP ... OB_MN, NONEBEAD), ion enum (ION_*), obSlot, ionTypeFromResname, ionMass, onebeadResidueAlias, isOnebeadProteinResidue, glycanTypeFromResname, isUnsupportedGlycan (static).
  - `OnebeadMapping(PDB template, atoms, Log&, ErrorHandler)`: bead of each atom, bead types, masses, weights (m_a/M_bead), CSR atoms of each bead, atom/residue names (residue after alias), glycan type and glycosidic-bridge flags, `getBeadPositions(pos, beads)` (threaded COM, same summation order).
  - ErrorHandler = std::function called with the message (SAXS passes Action::error); a helper throws if it returns.
- The per-atom bead typing was in getOnebeadparam, getOnebeadparam_sansH and getOnebeadparam_sansD as three identical ~680-line copies: now once, in the mapping. SAXSOnebead.cpp: 4842 -> 2362 lines (tables only).
- `src/isdb/LCPO.h/.cpp` (git rename of SAXSLCPO.cpp): `LCPO::getParameterTable()`, constructor from atom names, residue names, glycan and bridge flags (+ terminal corrections), `computeAreas(pos, first, stride, area)` with the cell-list Verlet list (MPI split by the caller). SAXS keeps only the per-bead reduction and the cutoff.
- SAXS inherits OnebeadTypes privately (enumerators in scope, tables unchanged); getIonData takes the mass from ionMass.
- Net: 1910 insertions, 2932 deletions.

## Verification (cloud)
- Bitwise identical to master: SAXS+SANS ONEBEAD, SOLVATION_CORRECTION with SOLVATION_STRIDE=1, SANS DEUTER_CONC=0.48, comp mode, values and DUMPDERIVATIVES at %.14e, protein + RNA + ions, 4 frames; serial and real MPI with 2 and 3 ranks (OpenMPI).
- Glycan template: bead of each atom, masses, glycan types, bridge flags, bead types and all LCPO parameters identical to master (temporary dumps, removed).
- 78/78 isdb regtests with OpenMPI (incl. all MPI ones); clang -Wall -Wshadow -Werror without OpenMP; gcc -Wextra on the new files; astyle.
- Only visible difference: for a residue unknown both to the bead typing and to the LCPO table, "Residue not known" now comes before the LCPO error.

## Findings
- Glycans are mapped and typed (and have LCPO parameters) on master, but there are no ONEBEAD form factors for GLYC_* types: SAXS/SANS with glycans stop with "ONEBEAD form-factor parameters are not available for bead type 40 in this build". No regtest covers glycans.
- sasa/sasa_LCPO.cpp has its own LCPO table (amino acids only; same numbers as the SAXS one), an O(N^2) neighbour list and std::find in the triple loop. Step 2 needs a derivative path in isdb::LCPO (SAXS uses areas only). Module dependency: sasa would need USE=isdb, or LCPO/OnebeadMapping move to a shared place. sasa is Andrea Arsiccio's module: keep the change minimal.
- Cloud environment: Ubuntu 24.04 mpich is built with PMIx, so hydra mpirun gives singletons (every rank size 1, MPI regtests fail with "invalid number of processes for multi environment"). Use OpenMPI (libopenmpi-dev openmpi-bin, OMPI_ALLOW_RUN_AS_ROOT=1 OMPI_ALLOW_RUN_AS_ROOT_CONFIRM=1, PLUMED_MPIRUN="mpirun.openmpi --oversubscribe"). Installing OpenMPI switches the mpicxx alternative: a tree configured with plain mpicxx must then use mpicxx.mpich explicitly.

## Next
- Carlo: review, push, PR.
- Step 2 (SASA_LCPO on the shared LCPO, with derivatives) and step 3 (mapping action) to be decided.
- Optional: a glycan regtest once glycan form factors exist.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e8e794c6c1cd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.