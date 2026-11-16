# Issue

**Title:** AST-related modifications

## Description

These were some stuff I had written previously and thought to finish.

### Deprecating astunparse

I've swapped out `astunparse` from anything `>= python 3.9` because `ast.unparse` is available in stdlib since python 3.9 (also `astunparse` is no longer actively supported).  Behavior is slightly different (i.e., not as verbose with parenthesis and linebreaks) so I've added/modified the existing tests to test both `astunparse` and `ast.unparse`.

### Removing the last of `ast.Num`/`ast.Str`

In #1838, most instances of `ast.Num`, `ast.Str` (and other deprecated classes) are swapped over to `ast.Constant`. There is this lone TODO where swapping to `ast.Constant` is too broad and breaks behavior. I believe it's intended to catch lone words that result from [this line](https://github.com/mdtraj/mdtraj/blob/master/mdtraj/core/selection.py#L78), which means it's a lone `ast.Constant` and not a "safe name". 

All tests passed and same behavior as before (v1.9.9) with all sorts of keywords thrown at it.

```
In [1]: from mdtraj.core.selection import parse_selection

In [2]: parse_selection('bar')
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
Cell In[2], line 1
----> 1 parse_selection('bar')

File ~/Documents/Important/Pitt/git/mdtraj/mdtraj/core/selection.py:418, in parse_selection.__call__(self, selection)
    415 # Special check for a single literal
    417 if isinstance(astnode, ast.Constant) and astnode.value not in {True, False, None}:
--> 418     raise ValueError(
    419         "Cannot use a single literal as a boolean. " f"Choked on node with value {astnode.value}",
    420     )
    422 args = [ast.arg(arg="atom", annotation=None)]
    423 signature = ast.arguments(
    424     args=args,
    425     vararg=None,
   (...)
    430     kw_defaults=[],
    431 )

ValueError: Cannot use a single literal as a boolean. Choked on node with value bar

In [3]: parse_selection('protein')
Out[3]: _ParsedSelection(expr=<function <lambda> at 0x103623a30>, source='atom.residue.is_protein', astnode=<ast.Attribute object at 0x1037318d0>)

In [4]: parse_selection('name CA')
Out[4]: _ParsedSelection(expr=<function <lambda> at 0x11dc65240>, source="atom.name == 'CA'", astnode=<ast.Compare object at 0x10362a080>)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `15514a57a98b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.