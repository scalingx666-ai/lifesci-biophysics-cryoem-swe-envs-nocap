# Issue

**Title:** Update `SoftAdapt` implementation

## Description

This is a small update to the implementation of `SoftAdapt` in `nequip`. 

Basically, `SoftAdapt` tends to end up giving a close to 1:1:1 E:F:S coefficient ratio in most cases (which also seems to be what the figure in the paper shows (below), where different initial ratios converge to 0.33:0.33:0.33 once SoftAdapt kicks in). In a lot of cases you might want this, and seems like a good starting point when you don't know what E:F:S ratio to choose, but it mostly makes the choice of E:F:S irrelevant. 

![image](https://github.com/user-attachments/assets/4dec76a7-7547-4dc2-b8d2-de22ca045138)

This PR adds a `weighted` option to the `SoftAdapt` implementation, which uses the chosed E:F:S loss coefficients as additional weights in the soft-adapt loss updates (essentially keeping the user-supplied E:F:S coefficients as the baseline/target, with the softadapt algorithm scaling from this point to adjust the loss coefficients accordingly). This results in training behaviour where the loss coefficients oscillate around this E:F:S set-point, rather than 1:1:1, and seems possibly beneficial over a fixed E:F:S choice.


## Types of changes
- [ ] Bug fix (non-breaking change which fixes an issue)
- [x] New feature (non-breaking change which adds or improves functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to change)
- [ ] Documentation improvement (updates to user guides, docstrings, or developer docs)

## Checklist:
- [x] I have read ["Contributing to NequIP"](../docs/dev/contributing.md).
- [x] My code follows the code style of this project and has been formatted using `black`.
- [x] All new and existing tests passed, including on GPU (if relevant).
- [ ] I have added tests that cover my changes (if relevant).
- [x] I have updated `CHANGELOG.md`.
- [ ] I have updated the documentation (if relevant).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `afd2317be52b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.