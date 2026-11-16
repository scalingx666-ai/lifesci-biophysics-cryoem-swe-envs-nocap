# Issue

**Title:** New path tree

## Description

This PR starts on a new path tree code. The main reason for this is that the old path trees do not fully work with SimStore because they rely on a bad design decision that SimStore is removing. Additionally, the old path trees don't work with two-way shooting in either SimStore or netcdfplus.

Furthermore, there is no unit testing for the old path trees, and they're written in high complexity code that would be difficult to test. There's also very little documentation to help anyone else maintain that code.

Plus, I think the new version has a much nicer API, especially for the most common use case (selecting replica 0):

```python
tree = PathTree(steps)
tree.matplotlib()
```

Also, the API for added new plotting utilities is very straightforward: you create a class that implements two functions:

* `draw_trajectory(row, step, options)`
* `draw_connector(x, bottom, top, step, options)`

In these `step` is a namedtuple that carries the `mover`, the `trajectory`, the bool `accepted`, and `segments`, which provides information about which parts of the trajectory were new and which existed in the prior trajectory. `options` us the object which carries all the user-customizable plotting options, also contains a function that will get the left and right endpoints of the trajectory. `row` is the row number of the trajectory. `x` is the location of the shooting point, and bottom and top are row numbers to connect the trajectory to the shooting point origin. Examples of this are in the `mpl.py` file.

Unfortunately, there's a lot of really good functionality in the old path trees that will take time to replicate. I'm curious if anyone has ever actually used some of those (like changing the CSS to use different colors, especially colors that vary across a trajectory, representing the values of the snapshot, or adding text labels per snapshot).

---

2024 UPDATE: Overview of the contents here:

* `path_tree.py`: Generate the objects that describe the path tree. This is content, as separated from presentation, but has conveniences for a few presentation modes.
* `options.py`: Attempt to generalize presentation options in a way that might be valid across multiple presentation tools. This is the part of the API that is most likely to undergo massive changes.
* `plotter.py`: ABC for presentation-specific plotting classes. This may see some future refactoring.
* `mpl.py`: Tools for using matplotlib for presentation.
* `svg.py`: Tools for using SVG for presentation.

The use modes that are expected to be supported are:

1. Use conveniences in the `PathTree` object to plot:

    ```python
    tree = PathTree(steps)
    tree.matplotlib()
    # or
    tree.svg()
    ```

2. Explicitly do content and presentation in separate steps:

    ```python
    # content for any presentation mode
    tree = PathTree(steps)
    # matplotlib presentation
    mpl_plotter = mpl.ThinLines()
    mpl_plotter.draw(tree)
    # svg presentation
    svg_plotter = SVGLines()
    svg_plotter.draw(tree)
    ```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2b764bbbaf20`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.