# Issue

**Title:** Control log file name via environment variable

## Description

Using `PLUMED_LOG_FILE` environment variable to control the log file name instead of being hardcoded to `PLUMED.OUT`

##### Description

Quick & Dirty way to control the log file name instead of being hardcoded to `PLUMED.OUT` is to use PLUMED_LOG_FILE env variable.
If the variable is set, use it, otherwise, use the default value.

##### Type of contribution

- [X] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [ ] changes to a module not authored by you
- [ ] new module contribution or edit of a module authored by you

##### Copyright

- [ ] I agree to transfer the copyright of the code I have written to the PLUMED developers or to the author of the code I am modifying.

##### Tests

- [ ] I added a new regtest or modified an existing regtest to validate my changes.
- [ ] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9b13f6fb4053`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.