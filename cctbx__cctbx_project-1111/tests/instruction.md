# Issue

**Title:** LCLS psana2 support

## Description

One of three pull requests to add psana2 support to cctbx.xfel and dials.  The others are:

dials/dials#3038
cctbx/dxtbx#860

For more information, see the dxtbx PR.

Specifics for cctbx:
- Use the psana2 api for reading the wavelength
- Add a check for LCLS UED data, which has no eBeam as of yet
- Switch to factor_ev_angstrom

Lots of folks worked on this, special thanks to @fredericpoitevin and @monarin!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4ae93e3d6672`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.