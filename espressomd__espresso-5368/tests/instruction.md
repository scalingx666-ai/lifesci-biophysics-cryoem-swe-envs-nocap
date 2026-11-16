# Issue

**Title:** Make all BreakageSpec parameters mandatory

## Description

BreakageSpec registered breakage_length and action_type via AutoParameters
but did not override do_construct, so the inherited ObjectHandle::do_construct
only set the parameters actually passed. Omitting breakage_length left the
core struct's value-initialized 0.0 in place; since the breakage check is
`distance >= spec.breakage_length`, that is always true and queues every bond
of that type for breakage on the first execute(), silently deleting them.

0.0 is a legitimate, tested breakage_length, so it cannot serve as an unset
sentinel. Following the established bonded-interaction convention
(ScriptInterface::Interactions::BondedInteraction::check_valid_parameters),
add a do_construct override that wraps construction in parallel_try_catch and
throws "Parameter '<name>' is missing" if any valid parameter is absent.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `62c745e1426a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.