# Issue

**Title:** Fixup for types assigned by SWC reader when soma is traced by a single point

## Description

In #582, @Kartik-Sama found an issue with the types (soma, axon,...) assigned by the SWC reader. In a few zoom-calls, we identified that the issue only occurs for _single-point_ soma reconstructions. The code used to be extremely hard to parse, but the central issue was that, if one had a single-point soma, the types would get appended twice, [here](https://github.com/jaxleyverse/jaxley/blob/8773b9d4bcf37164c31d8c0b6e7da3ee4cf280b8/jaxley/io/swc.py#L96) and [here](https://github.com/jaxleyverse/jaxley/blob/8773b9d4bcf37164c31d8c0b6e7da3ee4cf280b8/jaxley/io/swc.py#L102).

This PR fixes this. It also makes the function easier to parse and adds some documentation and testing. Big thanks to @Kartik-Sama for having identified the issue and helped a lot in closing it!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8524d940490d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.