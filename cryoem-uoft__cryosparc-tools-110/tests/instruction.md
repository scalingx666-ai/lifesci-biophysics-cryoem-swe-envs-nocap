# Issue

**Title:** fix(spm): show response data in API error messages

## Description

Example:
```py
In [3]: api.start_and_migrate(license_id='12312')
...
APIError: *** [API] (PUT http://cryoem0.sbi:61004/start, code 401) received error response
Response data:
{
    "detail": "Invalid License-Id header"
}
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a6b9f8d7b883`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.