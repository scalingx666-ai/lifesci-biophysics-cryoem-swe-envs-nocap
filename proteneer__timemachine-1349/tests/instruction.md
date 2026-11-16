# Issue

**Title:** Add atom mapping arguments to restrict the number of connected components in the core

## Description

Removes the `connected_core` option from `get_cores` and replaces with 2 more general options:

* `max_connected_components`: set to $k$, only allow cores with at most $k$ connected components. To recover the current behavior with `connected_core=True`, set to 1. For `connected_core=False`, set to `None`.
* `min_connected_component_size`: set to $n$, only allow cores with the size of the smallest connected component being $\geq n$

For now, keeps the current default behavior by setting `max_connected_components=1` and `min_connected_component_size=1` in `DEFAULT_ATOM_MAPPING_KWARGS`.

For equivalent settings, ~performance is slightly decreased (5-10% in my testing). This could probably be optimized more if necessary, but I was starting to hit diminishing returns (and think it might be preferable to prioritize clarity of the code over few-percent speedups here).~ After optimizations in  it's about 10% faster than the current implementation with default settings.

Increasing either `max_connected_components` or `min_connected_component_size` from their defaults appears to drastically reduce search space and improve performance in some examples. The intuition seems to be that allowing many smaller connected components means that we find a larger overall core (and thus fewer outer loops over decreasing core size), but are less able to prune the search tree at each iteration; the optimal values are likely somewhere in between.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3d18a73ae5bd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.