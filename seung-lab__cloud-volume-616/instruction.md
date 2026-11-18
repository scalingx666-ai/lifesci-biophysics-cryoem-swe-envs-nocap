# Issue

**Title:** Fix Bbox docstring, add example

## Description

This puts the docstring for Bbox in the right place so IDEs can display it, and adds a basic example

Before:
<img width="457" alt="Screenshot 2024-06-13 at 3 47 58 PM" src="https://github.com/seung-lab/cloud-volume/assets/4590343/83b41c51-a7b7-4564-b465-83103481da50">

After:
<img width="457" alt="Screenshot 2024-06-13 at 3 47 38 PM" src="https://github.com/seung-lab/cloud-volume/assets/4590343/c2e05536-779a-45bd-8c95-d065379fa2d3">

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1b9b1a15d5a7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.