# Issue

**Title:** feat: secure command-line login

## Description

Addresses #64

Run a login command that saves an authentication token to the user `~/.config` directory. This allows running cryosparc-tools scripts without storing plain-text passwords on disk or environment variables.

![Demo](https://github.com/user-attachments/assets/f4a9039f-872a-4ac9-b1e8-a0564194e65b)

```
❯ python -m cryosparc.tools login --help
usage: cryosparc.tools login [-h] [--url URL] [--email EMAIL] [--password PASSWORD] [--expires EXPIRES]

options:
  -h, --help           show this help message and exit
  --url URL            CryoSPARC web URL, e.g., http://localhost:39000
  --email EMAIL        login email, prompts when unspecified
  --password PASSWORD  login password, prompts when unspecified
  --expires EXPIRES    token expiration date in format YYYY-MM-DD, cannot be more than one year in the future, defaults to 14 days from now.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `14f20190f3b5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.