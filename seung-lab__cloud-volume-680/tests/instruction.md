# Issue

**Title:** Postgres performance improvements

## Description

This PR includes several performance enhancements for spatial index operations with a PostgreSQL backend.

  Key improvements:

   - Uses `UNLOGGED` tables to minimize disk I/O.
   - Implements server-side cursors with psycopg2 to avoid client-side buffering of large results, saving memory and speeding up
     queries.
   - Utilizes `copy_from` for faster bulk data insertion into the file_lookup table.
   - Sets `synchronous_commit=OFF` to accelerate write operations.
   - Dynamically calculates the chunk size for fetching index files based on the number of parallel workers.

  These optimizations provide a substantial performance boost for creating and querying spatial indexes in PostgreSQL

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9b53280a25ca`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.