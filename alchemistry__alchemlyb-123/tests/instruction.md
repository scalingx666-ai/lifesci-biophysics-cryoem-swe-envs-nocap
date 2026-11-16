# Issue

**Title:** fix some typo in visualization module

## Description

@orbeckst Sorry for trying to push this through. These changes are based on #120 and #121. I initially thought that they could go with the #114 but given that we want to push a 0.4.0 #122. I think it might be necessary to get this through.

Three things are accomplished in this PR.

- A unified way of representing units, which is discussed in #121 and I'm trying to propagate this to all other plotting methods. This is my fault as I initially thought that the internal unit would be the same as the input unit. fix #124
- A typo in the note part of the doc.
-  Allowing plot_ti_dhdl to use the method from separate_dhdl (#120). This could go with workflow but **the first two should go with 0.4.0**.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9b07c085c871`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.