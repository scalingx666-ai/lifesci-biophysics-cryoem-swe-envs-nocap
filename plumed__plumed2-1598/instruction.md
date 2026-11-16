# Issue

**Title:** Checkpoint final

## Description

# CHECKPOINT: final checkpoint at the end of the run (2026-11-09)

Branch `checkpoint-final` (from origin/master cc8c7193d): 2cdc1282d (final checkpoint), b81333e4f (AUTO reads it). Delivered to the Mac via bundles; to be pushed by Carlo. Issue text drafted for tracking engines other than GROMACS (no doCheckPoint signal, no end-of-run hook).

## Behaviour
- If the MD engine never sent `doCheckPoint` during the run, `cmd("runFinalJobs")` writes `plumed_final.cpt` (FILE with `_final` before the extension, like `_prev`; replica suffix after: `plumed_final.0.cpt`), before the final jobs run.
- It is the checkpoint of the next step: last step + difference between the last two steps presented (1 if only one).
- With the signal (GROMACS, which signals also at the last step) nothing is written: it would not match the engine's restart files.
- RESTART_FROM=AUTO reads the final checkpoint when it exists, unless the engine gave a different restart step (`setRestartStep`; the step stored in the final file is compared). driver now sends setRestartStep also with `--restart-from-checkpoint` (the step of that file), so a restart from an earlier STRIDE checkpoint reads FILE.
- The final checkpoint is backed up once a restart has restored a checkpoint (and by a new run), so it exists only after a run that ended normally.
- Only driver, simplemd and pesmd call runFinalJobs; no engine patch does. Documented in CHECKPOINT: engines without the signal should call runFinalJobs before finalizing. The destructor fallback (only if MPI is not finalized, try/catch) was not implemented (fragile).
- Lesson: making AUTO prefer the final file unconditionally broke 17 STRIDE restart tests (restart from step 21 after a normal end at 25): the engine's restart step must decide.
- Possible LAMMPS caveat (not checked): LAMMPS may present the last step again at restart (setup), so the N+1 checkpoint would not match (clear step-mismatch error).

## Code
- Checkpointing: `write(step,outPath,outPrevPath)`, `atRunEnd()`, static `taggedName`/`finalName`, members finalPath, signalReceived, lastStep, stepIncrement; restore() backs up finalPath. PlumedMain cmd_runFinalJobs calls `checkpointing.atRunEnd()` first. setup/Checkpoint.cpp: AUTO logic. Driver: setRestartStep with --restart-from-checkpoint.
- Docs: CHECKPOINT PLUMEDOC (keyword and text), CHANGES/v2.11.md.

## Verification
- rt-checkpoint-driver section F: run on 25 frames with no signal and no STRIDE -> plumed_final.cpt at step 25; restart with AUTO reads it -> COLVAR, COLVAR2 bitwise identical, old final backed up, new one for step 60; B (STRIDE) restarts from step 21 and AUTO reads plumed.cpt; with the signal (A) no _final file.
- Manual: 2 replicas (driver --multi 2) with AUTO -> plumed_final.0/1.cpt read, restart identical.
- All */rt-checkpoint*, */rt-restart*, */rt-h5-*, basic/rt-driver* pass (cloud, OpenMPI).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cc8c7193df04`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.