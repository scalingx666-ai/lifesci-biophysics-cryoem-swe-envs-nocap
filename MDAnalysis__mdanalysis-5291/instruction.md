# Issue

**Title:** Add ASV benchmark for Contacts analysis

## Description

## Summary
This PR adds an ASV benchmark for the Contacts analysis module.

The benchmark measures the runtime of `Contacts.run()` using the PSF/DCD dataset from MDAnalysisTests. This allows tracking
performance changes in the native contacts analysis workflow across MDAnalysis versions.

## Benchmark Added
analysis.contacts.ContactsBench.time_contacts_run

## Motivation
The Contacts analysis is commonly used to calculate native contacts in molecular dynamics trajectories. Adding a benchmark
helps detect performance regressions and measure improvements in future optimizations.

## LLM / AI generated code disclosure
LLMs or other AI-powered tools were used in this contribution: No

<img width="1276" height="771" alt="1" src="https://github.com/user-attachments/assets/1cdd6d0b-5256-45a6-9e3b-b9f9b4ace654" />

<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5291.org.readthedocs.build/en/5291/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3be91176336e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.