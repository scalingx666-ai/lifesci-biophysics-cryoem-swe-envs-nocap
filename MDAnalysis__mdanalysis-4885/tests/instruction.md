# Issue

**Title:** [fmt] Format all

## Description

Excluding the files touched by open PRs excluded in #4815, this PR removes the explicit inclusion of files, and formats the last remaining file. Only the explicitly ignored files are not formatted, and will be formatted once the associated PRs are merged.

## Developers certificate of origin
- [x] I certify that this contribution is covered by the LGPLv2.1+ license as defined in our [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE) and adheres to the [**Developer Certificate of Origin**](https://developercertificate.org/).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--4885.org.readthedocs.build/en/4885/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5eef34165b03`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.