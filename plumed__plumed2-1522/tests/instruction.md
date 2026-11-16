# Issue

**Title:** CI: run nightly on master, and add long regtests that run only there

## Description

##### Description

.

Runs the CI every night on `master`, and adds long regtests that run only there ( for the reasons).

- `ci.yml`: adds a daily `schedule` (02:37 UTC) and `workflow_dispatch`, so the nightly run is the normal CI and it can also be started by hand from the Actions tab.
- `linuxWF.yml`, `macWF.yml`: set `PLUMED_LONG_TESTS=yes` on scheduled and manually started runs, and `no` otherwise. A reusable workflow sees the caller's `github.event_name`, so this needs no new inputs.
- `linuxWF.yml`: the doc and coverage pushes (and the Codecov upload) are skipped on scheduled runs, because pushes to `master` already do them.
- `regtest/scripts/run`: a test with `plumed_long=yes` in its `config` is not run unless `PLUMED_LONG_TESTS=yes`. It prints `DEFERRED (long test, ...)` instead.
- `regtest/scripts/check`: counts such tests separately and reports "N long tests not run". They are neither errors nor "not applicable", so jobs with `PLUMED_ALL_TESTS=yes` stay green.
- The developer guide (`AddingRegressionTests.md`) and `CHANGES/v2.11.md` describe `plumed_long`.

No test is marked as long. Push and pull request runs behave as before.

Tested locally on two regtests, one of them marked `plumed_long=yes`, with `PLUMED_ALL_TESTS=yes`:
- without `PLUMED_LONG_TESTS`: "1 tests performed, 0 tests not applicable", "1 long tests not run", and `checkfail` passes;
- with `PLUMED_LONG_TESTS=yes`: "2 tests performed", and `checkfail` passes.

The three workflow files parse with PyYAML. The scheduled trigger itself can only run once this is on `master`.

##### Target release

I would like my code to appear in release __v2.11__

##### Type of contribution

- [X] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [ ] changes to a module not authored by you
- [ ] new module contribution or edit of a module authored by you

##### Copyright

- [X] I agree to transfer the copyright of the code I have written to the PLUMED developers or to the author of the code I am modifying.

##### Tests

- [ ] I added a new regtest or modified an existing regtest to validate my changes.
- [ ] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

Prepared with the help of Claude.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c2a5cd1525e2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.