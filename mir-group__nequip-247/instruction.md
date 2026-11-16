# Issue

**Title:** make dataset an optional argument for rescaling builder

## Description

## Description
Make dataset an optional argument for rescaling builders. so one can define the model from python api without errors like below
```python
 from nequip.utils import Config
 from nequip.model import model_from_config
 from nequip.scripts.train import default_config

 config = {}
 config.update(default_config)
 config.update(dict(
     l_max=2,
     parity=True,
     num_features=64,
     avg_num_neighbors=None,
     chemical_symbols = ["H", "C", "O"],
     num_basis=8,
     r_max=4.0,
     per_species_rescale_scales_trainable = True,
     per_species_rescale_shifts_trainable = False,
     global_rescale_scale = 1.0,
     per_species_rescale_shifts = 0.0,
     per_species_rescale_scales = 1.0,
 ))
 final_model = model_from_config(
         config=Config.from_dict(config), initialize=True, dataset=None,
 )
```


## Types of changes
<!-- What types of changes does your code introduce? Put an `x` in all the boxes that apply: -->
- [x] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds or improves functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to change)
- [ ] Documentation improvement (updates to user guides, docstrings, or developer docs)

## Checklist:
<!-- Put an `x` in all the boxes that apply. If you're unsure about any of
     these, don't hesitate to ask. We're here to help! -->
- [x] My code follows the code style of this project and has been formatted using `black`.
- [ ] All new and existing tests passed, including on GPU (if relevant).
- [ ] I have added tests that cover my changes (if relevant).
- [ ] The option documentation (`docs/options`) has been updated with new or changed options.
- [ ] I have updated `CHANGELOG.md`.
- [ ] I have updated the documentation (if relevant).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cea953c18266`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.