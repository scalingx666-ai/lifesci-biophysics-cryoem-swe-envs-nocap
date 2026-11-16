# Issue

**Title:** Add property accessors for retrieving HostGuest* molecules as OEMols

## Description

This PR allows retrieval of the `HostGuest*` host and guest molecules as `OEMol`s if the OpenEye toolkit is installed. If it isn't, an exception will only be raised if the properties are accessed.

Examples:
```python
>>> from openmmtools import testsystems
/Users/choderaj/github/choderalab/openmmtools/openmmtools/openmmtools/multistate/__init__.py:75: UserWarning: Warning: openmmtools.multistate API is experimental
  warnings.warn('Warning: openmmtools.multistate API is experimental')
>>> t = testsystems.HostGuestVacuum()
>>> t.host_oemol
<oechem.OEGraphMol; proxy of <Swig Object of type 'OEGraphMolWrapper *' at 0x1a21edf6c0> >
>>> t.guest_oemol
<oechem.OEGraphMol; proxy of <Swig Object of type 'OEGraphMolWrapper *' at 0x1a21fc3b70> >
>>> t.oemols
[<oechem.OEGraphMol; proxy of <Swig Object of type 'OEGraphMolWrapper *' at 0x1a21edf6c0> >, <oechem.OEGraphMol; proxy of <Swig Object of type 'OEGraphMolWrapper *' at 0x1a21fc3b10> >]
```

This also include some other cleanup:
- [x] Update to unreleased 0.19.0
- [x] Remove after_success script in travis since we no longer push `openmmtools-dev`
- [x]  
- [x] Clean up `HostGuest*` API to use `handle_kwargs` for more flexible override support

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3707920d82a7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.