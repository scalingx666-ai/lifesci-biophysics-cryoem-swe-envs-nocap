# Issue

**Title:** perf: faster PSQL ingestion and queries via parallelism

## Description

Improve psql performance for ingestion and query all spatial indices, multithread with optimized psql command, so psql can process data with multiple cores. Extended the query call with a `nthread` parameter to specify the number of thread used for psql in fast path. The change speed up the spatial index ingestion and mesh merge task generation from > 8 hours to < 2 hours.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e14416dcaeb6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.