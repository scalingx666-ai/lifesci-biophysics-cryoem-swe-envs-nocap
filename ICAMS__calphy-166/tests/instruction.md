# Issue

**Title:** Allow flexible ufm params

## Description

This pull request refactors the `Liquid` class in `calphy/liquid.py` to improve code readability and maintainability by reformatting multi-line code blocks and enhancing consistency in string formatting. The changes primarily involve the restructuring of method implementations and minor adjustments to logic.

### Code readability improvements:
* Reformatted multi-line method calls and string formatting for better readability, such as in `__init__`, `melt_structure`, `run_averaging`, and `run_integration` methods. [[1]](diffhunk://#diff-57754aa82b61708d302240d2f417fe4ab70b0c8b20b27b5f7d2fb47337e1153eR50-R66) [[2]](diffhunk://#diff-57754aa82b61708d302240d2f417fe4ab70b0c8b20b27b5f7d2fb47337e1153eL74-R86) [[3]](diffhunk://#diff-57754aa82b61708d302240d2f417fe4ab70b0c8b20b27b5f7d2fb47337e1153eL118-R150) [[4]](diffhunk://#diff-57754aa82b61708d302240d2f417fe4ab70b0c8b20b27b5f7d2fb47337e1153eL182-R209)

### Logical enhancements:
* Updated logic to use consistent string formatting (e.g., replacing single quotes with double quotes in `run_averaging` and `run_integration`). [[1]](diffhunk://#diff-57754aa82b61708d302240d2f417fe4ab70b0c8b20b27b5f7d2fb47337e1153eL118-R150) [[2]](diffhunk://#diff-57754aa82b61708d302240d2f417fe4ab70b0c8b20b27b5f7d2fb47337e1153eL182-R209)
* Improved the parameterization of the Uhlenbeck-Ford model in `thermodynamic_integration` by replacing hardcoded values with configurable attributes.

### Minor cleanup:
* Removed unnecessary blank lines and comments for cleaner code structure. [[1]](diffhunk://#diff-57754aa82b61708d302240d2f417fe4ab70b0c8b20b27b5f7d2fb47337e1153eL160-L163) [[2]](diffhunk://#diff-57754aa82b61708d302240d2f417fe4ab70b0c8b20b27b5f7d2fb47337e1153eL362-L363)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cab4b1f45792`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.