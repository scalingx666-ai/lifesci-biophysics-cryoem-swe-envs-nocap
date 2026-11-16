# Issue

**Title:** Add convert_to_cartesian for pure to Cartesian basis conversion

## Description

First part of #259, as described in my comment there.

`convert_to_cartesian(obasis)` in `convert.py` returns a new `MolecularBasis` in which all shells are Cartesian, and a sparse matrix `transform` so that `coeffs_new = transform @ coeffs_old`. It builds the blocks from `tfs` and handles the ordering and sign conventions of the old basis (e.g. the sign flipped f functions of ORCA) and of the new Cartesian functions. If the old conventions do not define some Cartesian ordering, the HORTON2 one is added.

The test checks it against `compute_overlap`, which is independent of the transformation. With two centers, pure and Cartesian shells, a generalized contraction and three different conventions, `transform.T @ S_cart` equals the overlap between the old and the new functions, and `transform.T @ S_cart @ transform` equals the old overlap. Together that means every old function is reproduced exactly by the new ones. I also checked that breaking the permutation handling makes the test fail.

Not in this PR, to keep it small: the `prepare_cartesian` function that applies `transform` to orbital coefficients and density matrices for the WFN and WFX dumpers. I will do it as a follow up if this API looks right.

Open question from the issue: I return a `scipy.sparse` array. Easy to change to dense if you prefer.

I used an LLM assistant (Claude Code) while working on this. The full test suite passes (`pytest iodata`), and `ruff check` and `ruff format` are clean.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4fa631838a44`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.