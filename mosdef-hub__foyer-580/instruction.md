# Issue

**Title:** Ensure XML files are opened with UTF-8 encoding

## Description

### PR Summary:

This PR ensures XML files are opened in UTF-8 mode while being processed. The background here is ... messy and hard to reproduce. What I'm fairly sure of is that my testing infrastructure writes some temporary files (including Foyer XML files) and something might be loaded under an assumption of ASCII encoding. This causes a crash similar to forcing that behavior:

```python
>>> open('oplsaa.xml', encoding='ascii').read()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/Users/mattthompson/micromamba/envs/new-models/lib/python3.11/encodings/ascii.py", line 26, in decode
    return codecs.ascii_decode(input, self.errors)[0]
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
UnicodeDecodeError: 'ascii' codec can't decode byte 0xce in position 56770: ordinal not in range(128)
```

This is because of the fancy (unicode) $\alpha$ [here](https://github.com/mosdef-hub/foyer/blob/3bb406337493e52d5890dcd6ecee1b1c76ceeb4b/foyer/forcefields/xml/oplsaa.xml#L683):

```python
In [1]: alpha = '\u03b1'

In [2]: alpha
Out[2]: 'α'

In [3]: alpha.encode("utf-8")
Out[3]: b'\xce\xb1'

In [4]: alpha.encode("ascii")
---------------------------------------------------------------------------
UnicodeEncodeError                        Traceback (most recent call last)
Cell In[4], line 1
----> 1 alpha.encode("ascii")

UnicodeEncodeError: 'ascii' codec can't encode character '\u03b1' in position 0: ordinal not in range(128)
```

AFAICT, Python 3 defaults to `open()`ing files in UTF-8 mode, so this change shouldn't impact anything but esoteric cases like I've run into. (Handwaving more, it's a fixture that crashes _only at collection time_ and only when using `pytest-xdist`.) This would be tricky for me to write a unit test for, but I can take a stab at it if desired.

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [x] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3bb406337493`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.