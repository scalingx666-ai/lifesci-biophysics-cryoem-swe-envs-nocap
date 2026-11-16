# Issue

**Title:** create_installer: call conda-pack directly

## Description

Since about 2022, conda discovers subcommands through a plugin system. By wiping the environment before attempting `conda pack` we are making the subcommand undiscoverable. Therefore we just directly call the standalone to bypass the plugin system.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5f35aae95f0a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.