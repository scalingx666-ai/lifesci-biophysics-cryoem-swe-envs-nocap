# Issue

**Title:** add `pip install packmol`

## Description

This PR introduces [cibuildwheels](https://cibuildwheel.pypa.io/en/stable/) to enable installation of packmol from PyPI.

There is currently no python interface beyond
```python
import packmol

packmol.__version__
```

but can in principle be expanded in the future.
The package has no external dependencies `dependencies = []` and should work on `[ubuntu-latest, ubuntu-24.04-arm, windows-latest, windows-11-arm, macos-15-intel, macos-latest]` and if `gfortran` is available on other platforms as well.

I've not included in the package description. Please let me know if I should update this?
```toml
authors = [
    { name = "<name>", email = "<optional: email>" },
]
```

The package will be build and  uploaded to PyPI on GitHub release actions (https://pypi.org/project/packmol/).
The only requirement being, to create an environment on GitHub for potential security features ->  `Settings -> Environments -> New Environment -> enter the name "pypi" -> configure environment` with no further additions necessary.
If you have or will create an account on PyPi I will transfer the full ownership.

With the popularity of https://docs.astral.sh/uv/ this will allow running packmol by just running `uvx packmol` in addition to having it as a dependency for other projects without relying on conda (my initial motivation).

The current version is set to `version = "0.1.0"`  in the `pyproject.toml` file which is the source of the version info for the python project.
Here is the release workflow associated with that version https://github.com/PythonFZ/packmol/actions/runs/19426176388
With e.g. `v21.1.2` this version could be removed in the future.

Happy to answer questions or adjust the code.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7cb4c1470652`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.