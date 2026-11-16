# Issue

**Title:** Dump read capability + bugfix.

## Description

1. The original Internal Field Separator is saved to $OIFS once and for all at the beginning of moltemplate.sh. The newline is defined as $CR, so that IFS can be set to $CR or $OIFS anywhere in the script.

2. In option -xyz, changed "NF==4" to "(NF>3) && (NR>2)" in order to be more tolerant with non-standard formats such as XYZQ.

3. in -pdb, I have fixed the formulas for the triclinic box: now it works again. Fixed typos in BOXSIZE_Y, BOXSIZE_XZ; cosmetic change in BOXSIZE_YZ and reverted to the wikipedia-style definition of BOXSIZE_Z, which yields smaller rounding errors.

4. Added the brand-new -dump option to read DUMP files, extracting: atomic coordinates, ellipsoid quaternions, and velocities. I have also changed the corresponding ellipsoid section in $OUT_DATA_FILE.

5. Added "Ellipsoids" to the list of sections in dump2data.py

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `53ff02ccb520`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.