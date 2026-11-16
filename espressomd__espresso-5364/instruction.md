# Issue

**Title:** Fix bond_breakage "none" specification

## Description

For an ActionType::NONE pair-bond queue entry, actions_for_breakage fell
through to the REVERT/angle-bond else branch and unconditionally evaluated
*(e.bond_partners[1]), which is std::nullopt for a pair bond. This is a
read of an empty std::optional (undefined behaviour). It traps under
_GLIBCXX_ASSERTIONS or a sanitizer, and even in a plain build it
dereferences the value-initialized payload 0, resolving to a real
non-virtual particle and emitting a spurious REVERT_BIND_AT_POINT_OF_
COLLISION runtime error (or, in other configurations, deleting the wrong
bond). NONE entries always queue because check_and_handle_breakage never
inspects action_type and breakage_length defaults to 0.0.

ActionType::NONE is a documented, valid configuration ("none" in the
Python/script-interface layer) and is the value of a default-constructed
BreakageSpec, so the correct behaviour is to do nothing. Add an early
return of an empty action set for ActionType::NONE at the top of
actions_for_breakage. This also resolves finding #34 (NONE on a virtual-
site angle bond silently deleting bonds via the same else branch).

Add a regression test (none_action_pair_bond_is_noop) that queues a NONE
pair-bond entry and checks process_queue is a clean no-op: no exception,
no runtime error, and the particles' bonds are preserved.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5f8956711645`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.