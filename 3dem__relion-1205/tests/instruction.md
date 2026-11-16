# Issue

**Title:** Fix motioncor2 1.6 parsing

## Description

This update addresses an issue with parsing global shift data when using MotionCor2 version 1.6, as described in . The current implementation relies on the version 1.5 parsing method, which fails to handle the updated format introduced in MotionCor2 version 1.6.

- Changes Introduced:
  - Fallback Parsing for Version 1.6 Data:
  - If no global shifts are detected using the version 1.5 method, the code now attempts to parse version 1.6 style data. This involves:
    - Resetting the input stream.
    - Searching for lines containing both "Frame" and "x Shift" to identify the relevant section.
    - Parsing and extracting `frame`, `x_shift`, and `y_shift` values.
    - Handling unexpected line formats gracefully, including detailed logging and error reporting.
  - Error Handling Enhancements:
    - Logs the filename (`fn_log`) when parsing errors occur.
    - Reports unexpected line formats with improved diagnostic information.
- Key Benefits:
  - Resolves , ensuring compatibility with the updated MotionCor2 version 1.6 data format.
  - Enhances robustness by implementing a fallback mechanism and improving error diagnostics.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `167a1749c8e9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.