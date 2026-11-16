# Issue

**Title:** ci: retry the autoconf download, and fall back to a GNU mirror

## Description

##### Description

`codecheck` currently fails outright when `ftp.gnu.org` is unreachable:

```
+ wget https://ftp.gnu.org/gnu/autoconf/autoconf-2.69.tar.gz
Connecting to ftp.gnu.org (ftp.gnu.org)|209.51.188.20|:443... failed: Connection timed out.
Connecting to ftp.gnu.org (ftp.gnu.org)|2001:470:142:3::b|:443... failed: Network is unreachable.
##[error]Process completed with exit code 4.
```

`.ci/install.autoconf` runs under `set -e`, so one failed connection ends the job and the pull request
goes red for a reason unrelated to its contents. Seen on #1468 and #1465.

Add `--tries=3 --timeout=30 --retry-connrefused`, and fall back to `ftpmirror.gnu.org`, which GNU
maintains as a redirect to a nearby mirror. Behaviour is unchanged when the first host answers.

##### Target release

I would like my code to appear in release __v2.10__

##### Type of contribution

- [X] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [ ] changes to a module not authored by you
- [ ] new module contribution or edit of a module authored by you

##### Copyright

- [X] I agree to transfer the copyright of the code I have written to the PLUMED developers or to the author of the code I am modifying.

##### Tests

- [ ] I added a new regtest or modified an existing regtest to validate my changes.
- [ ] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

<!-- No regtest: this is CI provisioning, which the regtest suite does not cover. The change is
exercised by codecheck itself on this PR. -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dbc9713a5061`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.