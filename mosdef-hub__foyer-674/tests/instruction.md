# Issue

**Title:** Speed up SMARTS atom typing (~2x)

## Description

## Summary

Three changes that speed up atom typing (`find_atomtypes`) by about 2x with OPLS-AA. The atom types produced are unchanged.

1. **Check atoms before graph structure in VF2** (`smarts_graph.py`): `SMARTSMatcher` overrides `syntactic_feasibility` so foyer's cheap atom-level SMARTS check (`node_match`) runs before networkx's induced-subgraph structure checks. Most candidate pairs fail on element or `X` count, so they skip the structural work. Pairs that pass go through the unchanged networkx check via `super()`. ~1.17x.
2. **Convert parse-tree tags to `str` once** (`smarts_graph.py`): `SMARTSGraph.__init__` converts each `Tree.data` from a lark `Token` to `str` after `_add_edges`, which still needs `.data.value`. `_atom_expr_matches` and `_atom_id_matches` compare these tags to string literals on every atom check. `Token.__eq__` is a Python-level call, while `str.__eq__` runs in C. This was about 15% of run time. ~1.8x on top of (1).
3. **Re-run only `%label` rules after the first pass** (`atomtyper.py`): rules without `%label` depend only on element, neighbor count, rings and bonds, none of which change during typing, so their matches are final after the first pass. Passes 2+ re-run only the 133 of 228 OPLS-AA rules that use `%label`. Convergence, `max_iter` and the warning behave as before. ~1.57x on top of (1) and (2).

### Combined (`main` vs. this branch, OPLS-AA)

Branched polyalkane chain (propyl branch on every other backbone carbon):

| Backbone C | Atoms | `main` | This PR | Speedup |
|-----------:|------:|-------:|--------:|--------:|
| 50 | 380 | 1.51 s | 0.74 s | 2.04x |
| 200 | 1,505 | 5.97 s | 2.86 s | 2.08x |
| 800 | 6,005 | 23.77 s | 11.21 s | 2.12x |
| 2,000 | 15,005 | 59.38 s | 27.96 s | 2.12x |

Atom types were identical to `main` at every size tested.

### Notes for reviewers

- `syntactic_feasibility` is a networkx method, not a documented hook. Its signature and its call order (just before `semantic_feasibility`) have been the same since well before our `networkx>=2.5` floor. The override can only reject pairs that `semantic_feasibility` would also reject, so results can't change. Pairs that pass the atom check run `node_match` twice; they are a small fraction of all candidate pairs.
- (2) changes `SMARTSGraph.ast` in place, so `Tree.data` is now `str` rather than `Token`. Inside foyer, `.data.value` is only read during graph construction. gmso, mbuild, forcefield-utilities and antefoyer don't use `SMARTSGraph.ast`.
- (3) relies on `%label` being the only SMARTS primitive that reads typing state. A comment in `_iterate_rules` says so and names recursive SMARTS (`$(...)`, not implemented yet) as the case to revisit.

## Tests

- New `TestIterateRules` in `test_atomtyping.py`:
  - a rule set that needs three productive passes, asserting the final whitelists and that the non-`%label` rule runs only once;
  - a check that the `max_iter` warning still fires.
- Full suite: 321 passed locally (Python 3.14, networkx 3.7), including the OPLS validation tests.
- The GAFF tests in `test_forcefield_parameters.py` are skipped without antefoyer. With antefoyer installed, their fixture fails on `forcefields.load_GAFF()` because #631 removed the plugin attachment in `foyer/forcefields/__init__.py`. That breakage is unrelated to this PR. I ran them with a local shim that re-attaches the entry points, and they pass. That should be fixed separately.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5ac2c17ac9a4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.