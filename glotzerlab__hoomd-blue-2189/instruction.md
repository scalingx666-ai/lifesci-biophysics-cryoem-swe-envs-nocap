# Issue

**Title:** Fix NaN error in table writing

## Description

## Description

Currently, NaN values cause the `hoomd.write.Table` to error when writing to output. The error happens in `write.table._Formatter.format_num()` because the formatter determines a minimum representation of the number by first converting it to `int`, which is not allowed for NaN. Here's a MWE:

```python
sim = hoomd.util.make_example_simulation()

logger = hoomd.logging.Logger(categories=["scalar"])
logger[("test", "nan")] = (lambda: float("NaN"), "scalar")

output = StringIO("")

table_writer = hoomd.write.Table(
    trigger=1, 
    logger=logger,
    output=output,
    delimiter=","
)

sim.operations.writers.append(table_writer)

sim.run(1)
```
which produces the following output:
```
  File "/home/joseph/micromamba/envs/gg/lib/python3.14/site-packages/hoomd/write/table.py", line 102, in __call__
    return self.format_num(value, column_width)
           ~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^
  File "/home/joseph/micromamba/envs/gg/lib/python3.14/site-packages/hoomd/write/table.py", line 130, in format_num
    min_len_repr = self._digits_from_decimal(value) + 1
                   ~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^
  File "/home/joseph/micromamba/envs/gg/lib/python3.14/site-packages/hoomd/write/table.py", line 110, in _digits_from_decimal
    digits = int(log10(abs(num)))
ValueError: cannot convert float NaN to integer
```

I have fixed this issue by adding an is-NaN check in `write.table._Formatter.format_num()`, which automatically returns the string `"NaN"` as the minimum representation.

## Motivation and context

While NaN values are almost never desired, they shouldn't cause the table writer to fail. I encountered this issue when writing code to probe the potential energy field around an ALJ particle, which is poorly defined when probing within the particle itself.

## How has this been tested?

I added a small unit test to cover this edge case.

No revisions to the documentation are needed.

## Checklist:

- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/CONTRIBUTING.rst).
- [x] I agree with the terms of the [**HOOMD-blue Contributor Agreement**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/ContributorAgreement.md).
- [x] My name is on the list of contributors (`sphinx-doc/credits.rst`) in the pull request source branch.
- [x] I have summarized these changes in `CHANGELOG.rst` following the established format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cfb1a83e2888`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.