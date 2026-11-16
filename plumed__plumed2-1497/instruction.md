# Issue

**Title:** kenref CI: pin KEnRef back to the v2.0.0 tag

## Description

##### Description

The follow-up promised in #1490 (step 5 of ): set the KEnRef pin in `.ci/install.kenref` back from the `release/2.x` branch to the `v2.0.0` tag.

#1490 needed a KEnRef change that was then only on KEnRef's `release/2.x`. After #1490 was merged, KEnRef's `master` was fast-forwarded and `v2.0.0` was moved to that commit (`de5385c`), so `release/2.x`, `master` and `v2.0.0` are now the same commit. CI therefore builds the same KEnRef library it built for #1490: the two commits added since (`5a01c96`, `de5385c`) change only docs, scripts, CMake comments and the opt-in PLUMED + GROMACS build (`KENREF_WITH_PLUMED_GROMACS`, off by default), none of which CI uses. Building this PLUMED against KEnRef `master` (configure's default) compiles again.

Checked locally: the workflow's resolve step maps `v2.0.0` to `de5385c`, and `.ci/install.kenref` builds and installs it with all three `.pc` files its check requires.

##### Target release

I would like my code to appear in release __master__

##### Type of contribution

- [ ] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [ ] changes to a module not authored by you
- [X] new module contribution or edit of a module authored by you

##### Copyright

- [X] I agree to transfer the copyright of the code I have written to the PLUMED developers or to the author of the code I am modifying.

##### Tests

- [ ] I added a new regtest or modified an existing regtest to validate my changes.
- [ ] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e5a05b184652`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.