# Issue

**Title:** Adding MD script, changing README, changing CLI structure

## Description

Hello,

I created a script that hides a lot of complexity of JAX MD to run NVT, NPT dynamics through the CLI using a `.yaml `config file. 

`md-so3lr --settings ./settings.yaml`

The user can choose the pre-trained model (default) or a custom one. Default settings are given in a `.md` file and in a string through calling `md-solr --help`. At least the initial geometry needs to be provided, then a short NVT run is started at 300K.

I also created a `cli` folder where the eval and md scripts can now be found. The project structure has been changed accordingly, so the behavior isn't altered.

The README is updated with a small description of this functionality.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c356bae26de9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.