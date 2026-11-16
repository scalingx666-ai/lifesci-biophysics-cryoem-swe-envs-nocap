# Issue

**Title:** fix: use unique logger name per job to prevent log handler conflicts

## Description

- logging.getLogger(__name__) returned same singleton for all jobs; each new job cleared previous job's file handler silently. Fixed by using log file path as unique logger name.
- Propagate MeltingTemp handlers to Solid/Liquid sub-job loggers so all sub-job output also appears in melting_temperature.log.
- Fix np.NaN -> np.nan for NumPy 2.0 compatibility.
- Add tests for no handler accumulation and no cross-contamination.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1acf9900ceca`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.