# Issue

**Title:** fix `SyntaxWarning: invalid escape sequence`

## Description

```
/opt/hostedtoolcache/Python/3.12.3/x64/lib/python3.12/site-packages/gromacs/fileformats/mdp.py:61: SyntaxWarning: invalid escape sequence '\s'
  COMMENT = re.compile("""\s*;\s*(?P<value>.*)""")  # eat initial ws
/opt/hostedtoolcache/Python/3.12.3/x64/lib/python3.12/site-packages/gromacs/fileformats/mdp.py:64: SyntaxWarning: invalid escape sequence '\s'
  """
/opt/hostedtoolcache/Python/3.12.3/x64/lib/python3.12/site-packages/gromacs/fileformats/ndx.py:89: SyntaxWarning: invalid escape sequence '\s'
  SECTION = re.compile("""\s*\[\s*(?P<name>\S.*\S)\s*\]\s*""")
/opt/hostedtoolcache/Python/3.12.3/x64/lib/python3.12/site-packages/gromacs/fileformats/xpm.py:122: SyntaxWarning: invalid escape sequence '\s'
  """\
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `463820c492fd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.