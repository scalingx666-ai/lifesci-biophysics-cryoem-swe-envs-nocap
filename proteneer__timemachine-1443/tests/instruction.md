# Issue

**Title:** Supports installing Timemachine from Tarball

## Description

Noticed that if attempted to install Timemachine from a tarball, it would fail to install on linux, as the C++ files were not included. This PR allow us to install from tarballs as well as to use pypi servers, if we wanted to. Previously we have only installed from a local git repo or by pip installing from the github url.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b0338073cb3f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.