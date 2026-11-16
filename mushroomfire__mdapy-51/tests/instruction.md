# Issue

**Title:** RingStatistics: a cancellation stops the search from one atom

## Description

`RingStatistics` searched one root per chunk and checked for cancellation only between chunks. For large rings in a dense network, one root takes minutes; the time grows about 3.5 times per ring size. In a random network of 300 atoms with coordination 13, rings up to 14 take about 40 s per root, and a cancellation after 0.3 s took 128 s to stop.

The search now checks the cancel token before each ring candidate and each node of the ball. When the token is set it stops, and `Cancelled` is thrown after the region. A chunk that returns early counts as finished, so without that throw the result would be quietly incomplete.

- Test: `test_a_cancel_stops_the_search_from_one_root`. It now stops within 0.3 s; 5 s is allowed.
- The other ring tests are unchanged: results with 1 and 4 threads are identical, and progress and a token cancelled before the start still work.
- GCC passes with -Wall -Wextra -Wpedantic -Wshadow -Werror.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dd58834dddd2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.