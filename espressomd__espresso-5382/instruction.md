# Issue

**Title:** Fix LBWalberla ghost communication

## Description

LB::LBWalberla::ghost_communication_pdf() forwarded to
lb_fluid->ghost_communication_vel() instead of
lb_fluid->ghost_communication_pdf() (a verbatim copy-paste typo from the
sibling vel wrapper, introduced in commit 5e39e84dc4 "Split LB
communicators"). The two backing methods use distinct communicators and
distinct lazy pending flags (GhostComm::PDF vs GhostComm::VEL).

Consequence: after a PDF-only setter such as set_node_density (which sets
ONLY GhostComm::PDF), the wrapper's mis-wired vel call sees VEL not
pending and no-ops, so the PDF ghost layer is never communicated. A
neighbouring rank then reads stale ghost densities (e.g. via the
CylindricalLBFluxDensityProfile observable on the setter-then-observe
path), and in an assert build the consider_ghosts PDF-pending assertion
in get_node_density fires.

Fix: forward to lb_fluid->ghost_communication_pdf(). The method has a
clear contract (flush the PDF ghost layer) with a correct backing
implementation; only the wrapper dispatched to the wrong one.

Adds a NUM_PROC 2 regression test exercising the CORE wrapper via
espresso::system->lb.ghost_communication_pdf(): write a distinct density
on every owned node, flush through the wrapper, and verify the ghost
layer on the neighbouring rank holds the communicated value. The
existing walberla_bridge tests cannot catch this because they call
LBWalberlaImpl::ghost_communication_pdf() directly, which is correct.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d2584a24bb12`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.