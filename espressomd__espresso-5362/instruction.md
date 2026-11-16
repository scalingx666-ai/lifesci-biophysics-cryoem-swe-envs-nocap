# Issue

**Title:** Make EnergyCriterion cut_off parameter mandatory

## Description

EnergyCriterion()'s core constructor takes a System argument, so the
core object is direct-initialized via make_shared<EnergyCriterion>(system)
with no value-initialization. The member m_cut_off had no in-class
initializer and was omitted from the ctor init list, and neither the
Python nor the script-interface layer enforced cut_off as a required
parameter. Constructing EnergyCriterion() without cut_off was therefore
accepted and left m_cut_off indeterminate; a subsequent get_params() or
decide() read uninitialized memory (nondeterministic clustering result).

Fix: add a do_construct override to the script-interface EnergyCriterion
that reads cut_off via the two-arg get_value<double>(params, "cut_off"),
matching the established required-parameter convention (BindCenters,
HollowConicalFrustum). Omitting cut_off now raises
RuntimeError "Parameter 'cut_off' is missing." cut_off is THE parameter
that defines the criterion, so prevention (required arg) is correct
rather than silently defaulting to a meaningless 0.0. As defense in
depth, also give the core member an in-class initializer
(double m_cut_off = 0.0;) so the raw read is well-defined regardless of
construction path.

Regression test added to testsuite/python/pair_criteria.py.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bb4aa33ecf75`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.