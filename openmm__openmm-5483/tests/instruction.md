# Issue

**Title:** Construct serialization child nodes in place

## Description

A small optimisation to replace children.push_back(SerializationNode()) with children.emplace_back() in SerializationNode::createChildNode(), avoiding construction and movement of a temporary node while preserving the API and resulting tree.

Benchmark: An MSVC x64 benchmark with 200,000 populated nodes measured one fewer allocation per node and approximately a 1.8% lower median construction time.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `685ded00ddb8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.