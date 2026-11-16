# Issue

**Title:** script_interface: fix LB node _population OOB read

## Description

The LBFluidNode "set_population" branch forwarded the user-supplied
vector to LBWalberlaBase::set_node_population verbatim, with no
length check at any layer (Python, SI, or core). The core
LBNodeAccess loop reads population[f] for f in [0, Stencil::Size)
(19 for D3Q19) via unchecked operator[], so a too-short vector (e.g.
lbf[0,0,0]._population = [1., 2.]) reads past the std::vector buffer,
injecting garbage populations or crashing. The slice path already
validates length via get_value_shape; the node path did not.

Fix: PREVENT the bad input. Before calling set_node_population, check
pop.size() == m_lb_fluid->stencil_size() inside parallel_try_catch so
the throw on the owning rank propagates uniformly to a Python
exception instead of an MPI desync. A malformed population vector is a
user error with no meaningful completion; padding/truncating would
silently inject incorrect physics. This mirrors the existing
checkpoint "population size mismatch" validation (LBFluid.cpp).

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dcc22ea93099`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.