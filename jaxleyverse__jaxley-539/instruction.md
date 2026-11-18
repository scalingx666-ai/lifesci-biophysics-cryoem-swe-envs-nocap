# Issue

**Title:** Small refactor of plotting

## Description

and refactors some plotting functionality. 
Changes:
- rm `morph_plot_kwargs`, rely on `**kwargs` instead.
- renamed kwarg `"col"` -> `"color"` for consistency with pyplot.
- Refactor `Network.vis`
 - cleanup of code (use new View functionality) and removal of unnecessary kwargs
 - rm `_to_graph` since it is obsolete now (replaced by scatter, which might be not as nice as the graph plotting, can be added back in though.). Since networkx is not used currently, this could also be removed as a dependency (until #355 makes it in at least)
 - disentangle moving and visualizing of cells, by adding `arrange_in_layers`. I think this is more clear and seperates out the additional kwargs of `vis`.

To get the same as `net.vis(layers=[3,3])` one now has to do.
```python
net.arrange_in_layers([3,3])
net.vis()
```

This now also works for both `detail="full"` and `detail="point"` and even for `dims=[0,1,2]` (not that this would be necessary). In general `network.vis` should feel more similar to `cell.vis`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dd477f27192d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.