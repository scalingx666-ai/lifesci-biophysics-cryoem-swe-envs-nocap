# Issue

**Title:** Align SpeciesConverter return type with TorchANI version

## Description

The mismatch was causing the README example to fail. The relevant TorchANI code is [here](https://github.com/aiqm/torchani/blob/46c554adc8b3b774c2e4172d645f456c28559d4b/torchani/nn.py#L154)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f927ebbaa9ab`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.