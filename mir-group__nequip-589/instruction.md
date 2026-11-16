# Issue

**Title:** Remove StateDict from torchsim integration

## Description

## Summary

`torch_sim.typing.StateDict` has been removed in . This causes an `ImportError` when loading the NequIP torchsim integration:

```
ImportError: cannot import name 'StateDict' from 'torch_sim.typing'
```

This PR updates `nequip/integrations/torchsim.py` to:
- Remove the `from torch_sim.typing import StateDict` import
- Change `forward(self, state: ts.SimState | StateDict)` → `forward(self, state: ts.SimState)`
- Replace the `isinstance` ternary that fell back to dict→SimState conversion with a direct `sim_state = state` assignment

## Test plan

- [ ] Verify NequIP torchsim integration tests pass against torch-sim main (post #487)

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `be54501ec853`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.