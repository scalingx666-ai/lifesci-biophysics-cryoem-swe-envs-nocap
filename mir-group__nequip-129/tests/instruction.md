# Issue

**Title:** Add minimal compatibility with vibes

## Description

[`vibes`](https://gitlab.com/vibes-developers/vibes/) is a toolkit for vibrational simulations with `ase`. This patch makes a very minimal change to allow `FHI-vibes` workflows to use the NequIP calculator. 

This does not address more subtle features, but it's good enough for experimental use. I'm not sure if this should be merged yet -- I just wanted to start the conversation. We'll be using this internally to explore NequIP in any case. :)

## Description

The only change is that it adds a function that takes a path to a deployed model file as input and returns the calculator. This is needed because the `vibes` infrastructure uses an input-file approach where you can define: a) the module from which to import a calculator, b) the name of the class/function that generates it, and c) a bunch of `kwargs` to be passed to the function or constructor in (b). Class methods can't be used in this way, so we can't make use of the `from_deployed_model` method. We may change this in the future, of course, but this is an easier fix for now.

The requisite input block for `vibes` looks like this:
```
[calculator]
name:                          nequip_calculator
module:                        nequip.ase.nequip_calculator

[calculator.parameters]
model:                         path/to/model.pth

```

## Motivation and Context
This enables `vibes` to use the NequIP calculator. Out of the box, this allows us to compute phonon band structures, run MD, do relaxations, etc. More complicated issues, such as stress, are not yet addressed.

## How Has This Been Tested?
I ran a short MD with a trained model and computed the VDOS compared to a baseline -- it looks fine. In fact, NequIP does very well! I think this change is small enough to not require extensive testing.

## Types of changes

- [ ] Bug fix (non-breaking change which fixes an issue)
- [x] New feature (non-breaking change which adds or improves functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to change)
- [ ] Documentation improvement (updates to user guides, docstrings, or developer docs)

## Checklist:
- [x] My code follows the code style of this project and has been formatted using `black`.
- [x] All new and existing tests passed.
- [ ] I have added tests that cover my changes (if relevant).
- [ ] The option documentation (`docs/options`) has been updated with new or changed options.
- [ ] I have updated `CHANGELOG.md`.
- [ ] I have updated the documentation (if relevant).

I consider this change to be too small to require inclusion in the changelog.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `af0d6c0811fd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.