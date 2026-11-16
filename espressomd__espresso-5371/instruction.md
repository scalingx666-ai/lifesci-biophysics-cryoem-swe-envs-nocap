# Issue

**Title:** analysis: reject calc_rh for chains shorter than 2 beads

## Description

The hydrodynamic radius is computed as a sum over distinct bead pairs
with a 0.5*N*(N-1) prefactor. For a single-bead chain (chain_length==1)
there are no pairs: the prefactor is 0 and the accumulated inverse
distance ri stays 0, so calc_rh evaluated prefac/ri = 0.0/0.0 = NaN and
returned a silent NaN array instead of reporting the ill-posed request.

check_topology only rejects chain_length<=0 and is shared with calc_re
and calc_rg, where single-bead chains are valid (R_e==0, R_g==0), so the
guard cannot live there. Add a calc_rh-specific precondition in the
script-interface dispatch that throws std::domain_error (surfaced as a
Python ValueError, consistent with the other chain-analysis domain
checks) when chain_length < 2.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `49ea40c288ba`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.