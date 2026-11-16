# Issue

**Title:** Fix PythonForce subset unwrap after periodic wrapping

## Description

.

`CommonCalcPythonForceKernel::getPositions` copies the whole-system cell-offset unwrap onto a PythonForce particle subset. Two independent defects sit on adjacent lines.

1. Index space. `posCellOffsets` is device-ordered. The whole-system path pairs `getPosCellOffsets()[i]` with `positions[order[i]]`. The subset path indexed it with `particles[i]`, an original particle id. `sortParticles` already builds `inverseOrder`.

2. Operator precedence. `positionsVec[i] -= a-b-c` parses as `pos - (a - b - c)` and therefore *adds* the y and z box vectors. The sibling form subtracts all three.

The dest copies that sibling subtraction (written so `-=` subtracts `a+b+c`) and the existing `inverseOrder` construction. It does not invent a new PBC helper.

Expression lattice, n_paired=8 mildly triclinic synthetic cells (labeled synthetic). Live `PythonForce.setParticles` wrap is Z on installed OpenMM 8.5.2 (subset API is master-only).

| quantity | max abs |
|---|---|
| minus-equals a-b-c vs sibling unwrap | 12.600000000000001 |
| minus-equals a+b+c vs sibling unwrap | 8.881784197001252e-16 |
| original-index vs device-index offsets | 2.0 |
| combined original-index and minus-equals vs sibling | 10.799999999999999 |
| combined device-index and sibling subtraction vs sibling | 8.881784197001252e-16 |

Native dest sha256 `92b25e45fcef6cf80e19f9d5550f40183da980d7843feea91f8996bbdb63440f` (`foliation-swe/aletheia-v1`). Applied with `patch -p1` without rewriting.

## AI Disclosure

This change was produced by Aletheia (a compiled counting-field model) as a unique live-frame copy of the sibling unwrap already in this file. I reviewed the dest against OpenMM `AI_POLICY.md`: it is a localized two-site rewrite, easy to verify, and does not introduce a large new block of generated code. I can answer questions about the index-space and `-=` parse. I will not use AI to reply to review questions.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e3f187424052`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.