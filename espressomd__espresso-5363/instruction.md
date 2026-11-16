# Issue

**Title:** Add feature guard to DipolarDirectSum

## Description

DipolarDirectSum::do_construct forwarded the gpu flag straight into the
core actor (m_is_gpu=true) with no feature check, unlike the analogous
CoulombP3M path. On a CUDA-off build this constructed and activated
silently; the first force/energy call then either hit assert(not
m_is_gpu) (debug) or silently computed on CPU while is_gpu() reported
True (release) -- a capability misreport.

Mirror the CoulombP3M convention: at the script-interface seam, if
gpu=True, add 'CUDA' to required_features and call
CodeInfo::check_features inside parallel_try_catch, so the unsupported
request surfaces as a clean RuntimeError ("Missing features CUDA")
before the core actor is created.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d9ea9ce502e6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.