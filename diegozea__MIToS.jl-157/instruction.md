# Issue

**Title:** Add Clustal format parser

## Description

## Summary
- add Clustal format with parser and printer
- expose new format in exports and docs
- document Clustal support
- test reading and writing Clustal alignments
- handle counts and conservation lines in Clustal files
- fix conservation parsing to track column annotations
- improve Clustal parser
- refine Clustal parser to better detect conservation lines

## Testing
- `julia --project -e 'push!(LOAD_PATH,"test"); using MIToSTests; MIToSTests.retest("MSA"); MIToSTests.retest("MSA")'` *(fails: BioAlignments package missing)*

------
https://chatgpt.com/codex/tasks/task_b_686a6c53e948832da98066f44495522e

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b63ab5a5db11`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.