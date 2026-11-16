# Issue

**Title:** Update Codecov action to v5

## Description

Update codecov action from `v4` to `v5` (changes deprecated variables to new values).

I suspect that the fix for #4576 would be to switch to [token-less access](https://docs.codecov.com/docs/codecov-tokens#uploading-without-a-token), but I need to understand this better.

PR Checklist
------------
 - [ ] Tests?
 - [ ] Docs?
 - [ ] CHANGELOG updated?
 - [ ] Issue raised/referenced?

## Developers certificate of origin
- [ ] I certify that this contribution is covered by the LGPLv2.1+ license as defined in our [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE) and adheres to the [**Developer Certificate of Origin**](https://developercertificate.org/).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--4887.org.readthedocs.build/en/4887/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d22272083967`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.