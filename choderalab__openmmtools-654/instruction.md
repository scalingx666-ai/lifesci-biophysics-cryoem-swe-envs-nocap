# Issue

**Title:** Not using zlib for varying length variables when storing in NetCDF

## Description

## Description

With newer NetCDF versions there is a problem with using `zlib=True` when storing variables with varying length. More info at 

## Todos
- [x] Implement feature / fix bug
- [ ] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst) to summarize changes in behavior, enhancements, and bugfixes implemented in this PR

## Status
- [x] Ready to go

## Changelog message
```
Support for newer NetCDF versions (>1.5.8) by not using zlib compression for varying length variables.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1abb4f8112d2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.