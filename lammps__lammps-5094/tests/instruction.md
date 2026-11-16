# Issue

**Title:** Change torch.load to include weights_only parameter

## Description

In trying to run `lmp -i in.mliap.pytorch.ace.NN` in the `examples/mliap` folder with the 4Jul2026 release and torch 2.13.0, I was getting the traceback below. This PR resolves that by passing `weights_only=False` to `torch` as done elsewhere in the codebase.

```
Traceback (most recent call last):
  File "mliap_model_python_couple.pyx", line 67, in mliap_model_python_couple.MLIAPPY_load_model
  File "/home/ROSENGROUP/software/lammps/pyenv/lib/python3.13/site-packages/torch/serialization.py", line 1609, in load
    raise pickle.UnpicklingError(_get_wo_message(str(e))) from None
_pickle.UnpicklingError: Weights only load failed. This file can still be loaded, to do so you have two options, do those steps only if you trust the source of the checkpoint.
        (1) In PyTorch 2.6, we changed the default value of the `weights_only` argument in `torch.load` from `False` to `True`. Re-running `torch.load` with `weights_only` set to `False` will likely succeed, but it can result in arbitrary code execution. Do it only if you got the file from a trusted source.
        (2) Alternatively, to load with `weights_only=True` please check the recommended steps in the following error message.
        WeightsUnpickler error: Unsupported global: GLOBAL fitsnap3lib.lib.neural_networks.write.TorchWrapper was not an allowed global by default. Please use `torch.serialization.add_safe_globals([fitsnap3lib.lib.neural_networks.write.TorchWrapper])` or the `torch.serialization.safe_globals([fitsnap3lib.lib.neural_networks.write.TorchWrapper])` context manager to allowlist this global if you trust this class/function.
```

**Summary**

<!--Briefly describe the new feature(s), enhancement(s), or bugfix(es) included in this pull request.-->

**Related Issue(s)**

<!--If this addresses an open GitHub issue for this project, please mention the issue number here, and describe the relation. Use the phrases `` or ``, when you want an issue to be automatically closed when the pull request is merged-->

**Author(s)**

<!--Please state name and affiliation of the author or authors that should be credited with the changes in this pull request. If this pull request adds new files to the distribution, please also provide a suitable "long-lived" e-mail address (ideally something that can outlive your institution's e-mail, in case you change jobs) for the *corresponding* author, i.e. the person the LAMMPS developers can contact directly with questions and requests related to maintenance and support of this contributed code.-->

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

<!--Please update the following sentence as needed in case you were using AI tools to
generate code.  Please mention which specific AI tool or tools you were using and which
parts of the code were AI generated versus the parts written by a human.  The use of AI
tools for checking spelling and syntax does not need to be reported.-->

@stanmoore1 used Claude Code to generate . 

**Backward Compatibility**

<!--Please state whether any changes in the pull request will break backward compatibility for inputs, and - if yes - explain what has been changed and why-->

**Implementation Notes**

<!--Provide any relevant details about how the changes are implemented, how correctness was verified, how other features - if any - in LAMMPS are affected-->

**Post Submission Checklist**

<!--Please check the fields below as they are completed **after** the pull request has been submitted. Delete lines that don't apply-->

- [ ] The feature or features in this pull request is complete
- [ ] Licensing information is complete
- [ ] Corresponding author information is complete
- [ ] The source code follows the LAMMPS formatting guidelines
- [ ] Suitable new documentation files and/or updates to the existing docs are included
- [ ] The added/updated documentation is integrated and tested with the documentation build system
- [ ] The feature has been verified to work with the conventional build system
- [ ] The feature has been verified to work with the CMake based build system
- [ ] Suitable tests have been added to the unittest tree.
- [ ] A package specific README file has been included or updated
- [ ] One or more example input decks are included

**Further Information, Files, and Links**

<!--Put any additional information here, attach relevant text or image files, and URLs to external sites (e.g. DOIs or webpages)-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c4fe7a5bcf91`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.