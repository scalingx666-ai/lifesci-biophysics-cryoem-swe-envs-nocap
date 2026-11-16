# Issue

**Title:** Added NoJump on-the-fly example to MSD documentation (Issue #4169)

## Description

This PR updates the MSD documentation inside `MDAnalysis.analysis.msd` to show users how to correctly apply the NoJump transformation using on-the-fly trajectory transformations.

Issue #4169 noted that although NoJump exists, there was no example showing how to use it directly in MDAnalysis.

This PR adds:
- A minimal working example demonstrating NoJump usage
- Clarification that wrapped coordinates must be unwrapped
- A modern MDAnalysis-based workflow that does not require external tools

Fixes: #4169
I would appreciate any feedback or suggestions for improvement.
Reviews are very welcome, and I’m happy to revise the PR based on your guidance.

<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5165.org.readthedocs.build/en/5165/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a029dcd240dc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.