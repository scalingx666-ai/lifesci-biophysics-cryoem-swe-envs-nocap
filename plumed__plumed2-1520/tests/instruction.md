# Issue

**Title:** kenref: follow KEnRef's stable branch by default

## Description

##### Description

KEnRef now has a `stable` branch, which its maintainers move only onto releases that passed KEnRef's full build and validation pipeline. This PR makes the kenref module build KEnRef from that branch by default:

- `configure --enable-kenref`, when `kenref_core` is not installed, now clones `stable` instead of `master` (KEnRef's development trunk). `KENREF_GIT_TAG=master` still builds the trunk, and/or any other branch or tag.
- the helper scripts in `src/kenref` (`build-only.sh`, `build-and-patch.sh`) use the same default.
- CI (`.ci/install.kenref`) builds `stable` instead of the fixed tag `v2.0.1`. The workflow already resolves the ref to a commit for the cache key (with a fallback for branches), so the cached install is rebuilt when `stable` moves and reused otherwise.
- `module.md` and `install.md` say what the default is.

`stable` is now at KEnRef v2.0.4.

The trade-off for maintainers: the KEnRef that PLUMED's CI builds can now change without a commit in PLUMED, when KEnRef moves `stable`. It is moved only after KEnRef's full pipeline passes on the new release, so this should not break CI, and it saves a PLUMED PR for every KEnRef release. If it ever does break CI, `KENREF_VERSION` in `.ci/install.kenref` can be set back to a tag.

Tested locally:
- the workflow's resolve step resolves `stable` to d8bc240 (v2.0.4), so the cache key names that commit.
- `.ci/install.kenref` clones `stable` (d8bc240), builds and installs `kenref_core` with all four `.pc` files.
- a clean `./configure --enable-kenref` clones `stable` and builds `kenref_core` and PLUMED with the kenref module.
- `KENREF_GIT_TAG=master ./configure --enable-kenref` clones `master` and configures as before.

##### Target release

I would like my code to appear in release __v2.11__

##### Type of contribution

- [ ] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [ ] changes to a module not authored by you
- [X] new module contribution or edit of a module authored by you

##### Copyright

- [X] the module I added or modified contains a `COPYRIGHT` file with the correct license information. Code should be released under an open source license. I also used the command `cd src && ./header.sh mymodulename` in order to make sure the headers of the module are correct.

##### Tests

- [ ] I added a new regtest or modified an existing regtest to validate my changes.
- [ ] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

Prepared with the help of Claude.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c2a5cd1525e2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.