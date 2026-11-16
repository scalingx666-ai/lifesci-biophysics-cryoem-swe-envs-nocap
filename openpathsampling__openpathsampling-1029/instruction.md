# Issue

**Title:** Make histogramming more robust

## Description

Issue found by a master student:
If `cv` singletons are not unpacked the `SparseHistogram` might return broken output bins, which then fail in following code that assumes it is 1D (Like in [this line](https://github.com/openpathsampling/openpathsampling/blob/aaff2b8e41cf42a13bbb5ce55a498ff66d03e8c6/openpathsampling/analysis/path_histogram.py#L210)).

This PR forces a reshape to prevent mangled output

master:
```python
>>> from openpathsampling.numerics.histogram import SparseHistogram
>>> histo = SparseHistogram(bin_widths=(0.5, 0.3), left_bin_edges=(0.0, -0.1)) 
>>> data = ([0.0],[1.0])
>>> histo.map_to_bins(data)
(array([0., 0.]), array([2., 3.]))
>>> max(histo.map_to_bins(data))
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: The truth value of an array with more than one element is ambiguous. Use a.any() or a.all()
```

This PR:
```python
>>> from openpathsampling.numerics.histogram import SparseHistogram
>>> histo = SparseHistogram(bin_widths=(0.5, 0.3), left_bin_edges=(0.0, -0.1)) 
>>> data = ([0.0],[1.0])
>>> histo.map_to_bins(data)
(0.0, 3.0)
>>> max(histo.map_to_bins(data))
3.0
```

It also fixes some pep8 issues in histogram.py

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `aaff2b8e41cf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.