# Issue

**Title:** analysis: accept p_type=-1 for center_of_mass and angular_momentum

## Description

The script-interface handlers for center_of_mass and angular_momentum
unconditionally called check_particle_type(p_type), which throws for any
p_type < 0. This rejected the documented and core-supported p_type == -1
sentinel ("all non-virtual particles"), so system.analysis.angular_momentum
(p_type=-1) and center_of_mass(p_type=-1) raised "Particle type -1 does not
exist" instead of computing the sum over all particles.

The core functions center_of_mass and angular_momentum already implement the
-1 case ((p.type() == p_type or p_type == -1)). Guard the validation so the
documented sentinel bypasses check_particle_type while explicit types are
still validated. The scope is intentionally limited to these two methods:
min_dist and distribution still (correctly) reject -1.

Also align the center_of_mass docstring with angular_momentum's already-correct
text mentioning the -1 sentinel, and add regression tests asserting the -1
result equals the sum over all non-virtual particles (distinct from the
type-0-only result, so a wrong-subset regression is also caught).

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ad29b7125f4d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.