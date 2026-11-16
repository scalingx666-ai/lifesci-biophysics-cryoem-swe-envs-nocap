# Issue

**Title:** MDCATH

## Description

This pull request introduces the MDCATH class, which extends torch_geometric.data.Dataset to facilitate the processing and handling of the mdCATH dataset, available on [Hugging Face](https://huggingface.co/datasets/compsciencelab/mdCATH).

Key Features:
- The MDCATH class processes the mdcath_source.h5 file, efficiently filtering domains and trajectories based on the specified filter arguments.
- Users can customize the filtering criteria by passing specific arguments to the class, allowing for targeted data selection

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6c42c8bfe467`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.