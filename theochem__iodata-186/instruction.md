# Issue

**Title:** Parse extended xyz

## Description

**Adds the functionality to read extended xyz files** (#151). The extended xyz format is described in detail in the [ASE documentation](https://wiki.fysik.dtu.dk/ase/ase/io/formatoptions.html#xyz). 

Functionality which is still missing is, compared to ASE: 
- Quoting values with [] or {} (only "" or '' is supported now).
- Parsing JSON content.

Implementing these features is a bit harder but I don't think they are essential.

For now, you can enable reading extended xyz-files by setting `atom_columns='EXT'` while loading a molecule.

```python
mol = load_one('a_file.xyz', atom_columns='EXT')
```

It is also possible to enable reading extended xyz files by default and falling back to reading normal xyz-files with when parsing fails. If needed, I can add this functionality.

Keywords which can be mapped to the IOData attributes are automatically stored in their appropriate attribute. However, it appears that there is still a bug when the `charge` attribute is present. I have mapped the charge keyword directory to the charge attribute of IOData (representing the total charge). The following code will fail:

```python
import iodata
mol = iodata.load_one('water_extended_trajectory.xyz', atom_columns='EXT')
print(mol.charge)
```

with the following error

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/home/maarten/anaconda3/envs/iodata/lib/python3.8/site-packages/iodata/iodata.py", line 274, in charge
    return self.atcorenums.sum() - self.nelec
TypeError: unsupported operand type(s) for -: 'float' and 'NoneType
```

The file `water_extended_trajectory.xyz` can be found in the data directory of the tests. Calling `mol.charge` sets the value of `atcorenums` (which is not present in the xyz file) equal to `atnums`. Next, IOData tries to calculate the total charge by substracting `nelec`, which is None leading to the error above. Should this be fixed? Or should I unmap the `charge` attribute?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e247a0a72571`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.