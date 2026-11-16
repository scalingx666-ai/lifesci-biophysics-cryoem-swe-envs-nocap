# Issue

**Title:** Drop unused newrelic dependency

## Description

Removes `newrelic` from `pyproject.toml` and `requirements.txt`. Nothing imports it, and on Python 3.12 it breaks `pip install` on macOS and Windows without a C compiler: no release since 12.0 ships cp312 wheels for those platforms, and the sdist build fails.

Edison literature queries run without warnings with or without it. Both dependency lists resolve for Python 3.12 on Windows and Apple-silicon macOS with no newrelic pulled in transitively (`edison-client` 0.18.0).

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ce9b1fab79e9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.