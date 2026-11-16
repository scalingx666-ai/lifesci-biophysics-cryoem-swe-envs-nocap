# Issue

**Title:** refactor: removed arbitrary arguments from AST module classes

## Description

This removes the arbitary arguments passed to `ast.Name` and `ast.Call` in the selection module and addresses:  

Support for arbitrary arguments will be deprecated in Py3.15.  

Changes are self-explanatory. 

Before and after tests are shown below: 

## Before

```bash
============================= test session starts ==============================
platform darwin -- Python 3.13.2, pytest-8.3.5, pluggy-1.5.0 -- /Users/*****/.local/share/virtualenvs/mdtraj-tICb6qJl/bin/python
cachedir: .pytest_cache
rootdir: /Users/*****/Library/CloudStorage/GoogleDrive-*****/My Drive/Software/mdtraj
configfile: pytest.ini
plugins: xdist-3.6.1
collecting ... collected 25 items

tests/test_selection.py::test_range PASSED                               [  4%]
tests/test_selection.py::test_unary_2 PASSED                             [  8%]
tests/test_selection.py::test_unary_3 PASSED                             [ 12%]
tests/test_selection.py::test_binary_1 PASSED                            [ 16%]
tests/test_selection.py::test_binary_2 PASSED                            [ 20%]
tests/test_selection.py::test_simple PASSED                              [ 24%]
tests/test_selection.py::test_alias PASSED                               [ 28%]
tests/test_selection.py::test_unary_1 PASSED                             [ 32%]
tests/test_selection.py::test_binary_selection_operator PASSED           [ 36%]
tests/test_selection.py::test_raises PASSED                              [ 40%]
tests/test_selection.py::test_raises2 PASSED                             [ 44%]
tests/test_selection.py::test_bool PASSED                                [ 48%]
tests/test_selection.py::test_nested_bool PASSED                         [ 52%]
tests/test_selection.py::test_values PASSED                              [ 56%]
tests/test_selection.py::test_element PASSED                             [ 60%]
tests/test_selection.py::test_not PASSED                                 [ 64%]
tests/test_selection.py::test_re PASSED                                  [ 68%]
tests/test_selection.py::test_within SKIPPED (failed to parse selection) [ 72%]
tests/test_selection.py::test_quotes PASSED                              [ 76%]
tests/test_selection.py::test_top PASSED                                 [ 80%]
tests/test_selection.py::test_top_2 PASSED                               [ 84%]
tests/test_selection.py::test_backbone PASSED                            [ 88%]
tests/test_selection.py::test_sidechain PASSED                           [ 92%]
tests/test_selection.py::test_literal PASSED                             [ 96%]
tests/test_selection.py::test_in PASSED                                  [100%]

=============================== warnings summary ===============================
mdtraj/core/selection.py:56
  /Users/*****/Library/CloudStorage/GoogleDrive-*****/My Drive/Software/mdtraj/mdtraj/core/selection.py:56: DeprecationWarning: Name.__init__ got an unexpected keyword argument 'SINGLETON'. Support for arbitrary keyword arguments is deprecated and will be removed in Python 3.15.
    THIS_ATOM = ast.Name(id="atom", ctx=ast.Load(), SINGLETON=True)

mdtraj/core/selection.py:57
  /Users/*****/Library/CloudStorage/GoogleDrive-*****/My Drive/Software/mdtraj/mdtraj/core/selection.py:57: DeprecationWarning: Name.__init__ got an unexpected keyword argument 'SINGLETON'. Support for arbitrary keyword arguments is deprecated and will be removed in Python 3.15.
    RE_MODULE = ast.Name(id="re", ctx=ast.Load(), SINGLETON=True)

tests/test_selection.py::test_re
tests/test_selection.py::test_re
  /Users/*****/Library/CloudStorage/GoogleDrive-*****/My Drive/Software/mdtraj/mdtraj/core/selection.py:199: DeprecationWarning: Call.__init__ got an unexpected keyword argument 'starargs'. Support for arbitrary keyword arguments is deprecated and will be removed in Python 3.15.
    left=ast.Call(

tests/test_selection.py::test_re
tests/test_selection.py::test_re
  /Users/*****/Library/CloudStorage/GoogleDrive-*****/My Drive/Software/mdtraj/mdtraj/core/selection.py:199: DeprecationWarning: Call.__init__ got an unexpected keyword argument 'kwargs'. Support for arbitrary keyword arguments is deprecated and will be removed in Python 3.15.
    left=ast.Call(

-- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html
=========================== short test summary info ============================
SKIPPED [1] tests/test_selection.py:267: failed to parse selection
================== 24 passed, 1 skipped, 6 warnings in 1.30s ===================
```

## After

```bash
============================= test session starts ==============================
platform darwin -- Python 3.13.2, pytest-8.3.5, pluggy-1.5.0 -- /Users/*****/.local/share/virtualenvs/mdtraj-tICb6qJl/bin/python
cachedir: .pytest_cache
rootdir: /Users/*****/Library/CloudStorage/GoogleDrive-*****/My Drive/Software/mdtraj
configfile: pytest.ini
plugins: xdist-3.6.1
collecting ... collected 25 items

tests/test_selection.py::test_range PASSED                               [  4%]
tests/test_selection.py::test_unary_2 PASSED                             [  8%]
tests/test_selection.py::test_unary_3 PASSED                             [ 12%]
tests/test_selection.py::test_binary_1 PASSED                            [ 16%]
tests/test_selection.py::test_binary_2 PASSED                            [ 20%]
tests/test_selection.py::test_simple PASSED                              [ 24%]
tests/test_selection.py::test_alias PASSED                               [ 28%]
tests/test_selection.py::test_unary_1 PASSED                             [ 32%]
tests/test_selection.py::test_binary_selection_operator PASSED           [ 36%]
tests/test_selection.py::test_raises PASSED                              [ 40%]
tests/test_selection.py::test_raises2 PASSED                             [ 44%]
tests/test_selection.py::test_bool PASSED                                [ 48%]
tests/test_selection.py::test_nested_bool PASSED                         [ 52%]
tests/test_selection.py::test_values PASSED                              [ 56%]
tests/test_selection.py::test_element PASSED                             [ 60%]
tests/test_selection.py::test_not PASSED                                 [ 64%]
tests/test_selection.py::test_re PASSED                                  [ 68%]
tests/test_selection.py::test_within SKIPPED (failed to parse selection) [ 72%]
tests/test_selection.py::test_quotes PASSED                              [ 76%]
tests/test_selection.py::test_top PASSED                                 [ 80%]
tests/test_selection.py::test_top_2 PASSED                               [ 84%]
tests/test_selection.py::test_backbone PASSED                            [ 88%]
tests/test_selection.py::test_sidechain PASSED                           [ 92%]
tests/test_selection.py::test_literal PASSED                             [ 96%]
tests/test_selection.py::test_in PASSED                                  [100%]

=========================== short test summary info ============================
SKIPPED [1] tests/test_selection.py:267: failed to parse selection
======================== 24 passed, 1 skipped in 0.81s =========================
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a970fbe25dc2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.