# Issue

**Title:** Restore zero Lepton derivatives for step and delta

## Description

See comments from codex below

PLUMED's matheval-compatibility changes made step differentiate to delta and delta to nandelta. At zero this injects infinity or NaN into forces, including products such as x^2*step(x) that have a finite first derivative. Restore original Lepton's constant-zero derivative rules, ignoring the singular impulses at discontinuities. Preserve explicit delta/nandelta function values and nandelta's existing derivative behavior.

Document the convention and adjust the two affected cells in each Lepton driver reference. Add independent first/second derivative expectations at signed zero, on both sides of the jump, and for an absent differentiation variable. Run the same checks in the interpreter and JIT, including explicit function-value compatibility checks.

Regenerate the import patch against pinned OpenMM 8.6.1. Also record the existing ParsedExpression null check already present in master but missing from that patch, so reimport reproduces all 17 vendored source files exactly.

Validation on native Apple Silicon with Apple Clang and ARM64 AsmJit:
- Both new regression modes fail before the fix at step's derivative at -0.
- rt-lepton, rt-lepton-asmjit, rt-make-lepton-jit, rt-make-lepton-interpreter and rt42 pass after the fix.
- Basic suite: 421 pass, 23 dependency skips; the remaining gzip fixture passes after enabling zlib and rebuilding the affected translation units.
- Reimport matches all vendored sources byte for byte; direct plumedcheck for Custom.cpp and git diff --check pass. Hosted CI remains pending.

.

Agent: Codex (/root); no other agents collaborated.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cc88c9d4f1d7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.