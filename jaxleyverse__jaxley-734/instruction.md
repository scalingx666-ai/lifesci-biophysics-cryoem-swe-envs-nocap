# Issue

**Title:** Make build_dynamic_state_utils compatible with networks

## Description

We should not filter `NaN`s and branch points out of `jaxnodes.edges` / synapses: 

but just use tree_map helper functions that only apply the filtering functions `fn` to leaves with `keys` in `valid_keys` (those that are in `jaxnodes.keys()`)

```python
def tree_map_leaves_with_valid_key(tree, fn, valid_keys=None):
    """
    Apply fn(leaf) selectively to leaves that satisfy is_valid_membrane_leaf(key, leaf, valid_keys).
    """
    return tree_map_with_path(
        lambda path, leaf: (
            fn(leaf)
            if is_valid_membrane_leaf(get_key_name(path), leaf, valid_keys)
            else leaf
        ),
        tree,
    )
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9fb6f05b7311`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.