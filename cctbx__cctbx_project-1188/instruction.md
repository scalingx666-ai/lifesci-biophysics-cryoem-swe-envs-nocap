# Issue

**Title:** Xfel gui sacct fix

## Description

Reduce Slurm load from the xfel GUI job monitor

Summary

The cctbx.xfel GUI's JobMonitor thread polls the status of submitted processing jobs. Its previous implementation ran a separate sacct subprocess for every submission id of every active job, every 5 seconds, querying Slurm's accounting database (slurmdbd). On the shared NERSC cctbx account this produced ~6 sacct queries/second sustained, slowing Slurm cluster-wide.

This PR reworks how the GUI tracks job status to put minimal, bounded load on Slurm.

Changes

- Poll the Slurm controller instead of the accounting database. Live status now comes from a single squeue call per cycle (in-memory slurmctld state — cheap), with a single batched sacct used only to resolve the final outcome of jobs that have already left the queue. In steady state this places essentially zero load on slurmdbd.
- One query per cycle, not one per job. All active jobs are queried together rather than spawning a subprocess per submission id. Finished jobs (including TIMEOUT) are not re-queried.
- Sensible status for multi-part (ensemble) jobs. Jobs split across several sub-jobs now report a derived status (RUNNING if any part runs, FAILED if any part fails, DONE only when all finish) instead of showing "unknown"; finished sub-jobs are cached and not re-queried.
- Bulk DB writes and reduced UI churn. Status changes for a cycle are written to the application database in a single UPDATE, and the Jobs tab is redrawn only when something actually changed.
- Poll interval relaxed from 5s to 10s.

Net effect: from dozens of accounting-DB queries every few seconds to ~one controller query per 10s, independent of job count.

Files

- xfel/ui/components/submission_tracker.py — squeue-based batched query with sacct terminal-state fallback; ensemble aggregation; per-chunk terminal cache; shared
state-mapping helper.
- xfel/ui/db/xfel_db.py — update_job_statuses() bulk status-update helper.
- xfel/ui/components/xfel_gui_init.py — JobMonitor reconcile loop (batched query, bulk write, delta-only refresh).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `24170591a110`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.