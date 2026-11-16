# Issue

**Title:** Cache de-stringifying quantities

## Description

Most of the time loading a force field, particularly a large one, is spent de-stringifying quantities, i.e. converting `"1.2 angstrom"` to a corresponding `Quantity` object. This process is not slow at the scale of making a few `Quantity` objects but it is fairly slow at the scale of loading an entire force field. Some identical strings are passed through this process many times, making it an appealing candidate for caching. There might be speedups in other parts of the toolkit, but I haven't thought of and confirmed other cases yet. Only certain functionality is actually wired through `string_to_quantity` (it seems force field loading and de-serializing constrained atom distances?) so the impact won't necessarily be wide.

I'm seeing some modest speedups loading mainline force fields and a huge speedup in loading the ff14SB port. I think this should scale well with force field size, especially cases in which many parameters have some identical attributes in common (i.e. virtual sites with zeroed vdW terms or, more commonly, torsions periodicity/phase).

In:
```python
import warnings

warnings.filterwarnings('ignore')

from openff.toolkit import ForceField
from IPython import get_ipython

print("Loading Sage:")
get_ipython().run_line_magic(
    "timeit",
    "ForceField('openff-2.2.1.offxml')",
)

print("Loading Protein-Specific 0.0.3:")
get_ipython().run_line_magic(
    "timeit",
    "ForceField('Protein-Specific-0.0.3.offxml')",
)

print("Loading ff14SB:")
get_ipython().run_line_magic(
    "timeit",
    "ForceField('ff14sb_off_impropers_0.0.3.offxml')",
)
```
Out:
```shell
$ git checkout 0.16.6 && ipython compare.py               14:25:31  ☁  0.16.6 ☂
HEAD is now at 53f129e1 Defer/fix some minor import issues (#1959)
Loading Sage:
121 ms ± 4.8 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
Loading Protein-Specific 0.0.3:
331 ms ± 8.79 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
Loading ff14SB:
2.08 s ± 132 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
(openff-toolkit-test) [openff-toolkit]
$ git checkout cache-destringifying-quantity && ipython compare.py
Previous HEAD position was 53f129e1 Defer/fix some minor import issues (#1959)
Switched to branch 'cache-destringifying-quantity'
Loading Sage:
89.4 ms ± 1.28 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)
Loading Protein-Specific 0.0.3:
250 ms ± 16 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
Loading ff14SB:
804 ms ± 42.5 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```
Seeing similar timing with 50433bfb0349b8f6de30c5bfeaef9cb44a78cb22

A less-robust timing is how long it takes the test suite to run and I'm seeing CI finish in about  10-20 less time than recent runs on the `main` branch.

The inputs to this function are very well-defined; my understanding of cache collisions is that they tend to sneak in with complex objects, often containing many optional attributes, can functionally be hugely different due from only minute property differences, i.e. `Molecule` objects with different atom ordering. I have a hard time imagining how identical strings could be expected to produce different outputs.

If I understand caching correctly, this decorators creates a mapping under the hood between inputs and outputs as the function is evaluated. In other words, they keys are the argument inputs and in this case that's only ever a string (which looks like a quantity).

- [ ] Tag issue being addressed - can't find one, but of general interest
- [ ] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [x] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://open-forcefield-toolkit.readthedocs.io/en/latest/developing.html#style-guide) codebase
- [ ] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ef8e07a862b1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.