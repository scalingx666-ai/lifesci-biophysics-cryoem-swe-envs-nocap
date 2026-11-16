# Issue

**Title:** Predict more spots on stills, gather more signal.

## Description

For stills integration, after the time-dependent ensemble refinement, we reintegrate the data in the cctbx.xfel.mpi_integrate program.  Here, allow the option of expanding the Nave-parameter envelope that is used to predict spots, in order to capture more signal. Dramatically increase the overall multiplicity, possibly by a factor of 1.5. Use this phil parameter to activate this feature:
integration.recruitment.expand_nave_parameters=True

XFEL group is invited to compare before/after merging logs on Perlmutter:
cd /global/cfs/cdirs/lcls/sauter/LY99/mfx101080424/redoscamerge
vimdiff 42915231/*main* 44977789/*main*

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `09bc17a40132`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.