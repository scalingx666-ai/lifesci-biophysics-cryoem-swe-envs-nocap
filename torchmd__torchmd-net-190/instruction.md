# Issue

**Title:** Wandb resume

## Description

In certain scenarios, there is a need to resume training without creating a new wandb logger, for example killed or crashed process. The purpose of this pull request is to incorporate this functionality directly into the tochmd-net code, enabling users to have this possibility directly from the YAML conf file. To achieve this, two new arguments have been introduced: 

- `wandb-resume` (boolean); 
- `wandb-id`, the ID of the run to be resumed (it's possible to retrieve it from the project table on the wandb-page);

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `070a3b30e6e1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.