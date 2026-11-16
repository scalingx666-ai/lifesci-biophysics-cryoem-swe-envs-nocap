# Issue

**Title:** parquet parser for dataframe serialisation

## Description

This PR add a parquet parser, which will load the serialised dataframe and permits the usage of

```
    from alchemlyb.parsing.parquet import extract_dHdl, extract_u_nk
    import pandas as pd

    u_nk.to_parquet(path='u_nk.parquet', index=True)
    dHdl.to_parquet(path='dHdl.parquet', index=True)

    new_u_nk = extract_u_nk('u_nk.parquet', T=300)
    new_dHdl = extract_dHdl('dHdl.parquet', T=300) 
```
Fix

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `064c4fe525cf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.