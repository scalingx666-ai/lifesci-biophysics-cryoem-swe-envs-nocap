# Issue

**Title:** feat: Add "unit" to Bbox + accept physical Bbox in CV image functions

## Description

This PR adds `unit='vx'` as a parameter to the Bbox class.

Supported units: vx (voxels), pm (picometers), nm (nanometers), um (micrometers), ...etc

The default unit Neuroglancer works in is nanometers so nm is the default assumption when physical units are used. vx is the default unit for Bbox.

```python
from cloudvolume import Bbox

Bbox([0,0,0], [1,1,1], unit='vx')
Bbox([0,0,0], [1,1,1], unit='nm')
Bbox([0,0,0], [1,1,1], unit='um')

bbox = Bbox.from_delta([106820, 84304, 21360], 512) # units "vx" by default
print(bbox)
>>> Bbox([106820, 84304, 21360],[107332, 84816, 21872], dtype=np.int32, unit='vx')

# resolution units are nm by default
physical_bbox = bbox.convert_units("nm", resolution=[8,8,40], resolution_units="nm") 
print(physical_bbox)
>>> Bbox([854560, 674432, 854400],[858656, 678528, 874880], dtype=np.int64, unit='nm')

cv = CloudVolume(..., mip=2)
image = cv[physical_bbox] # works for any mip
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d4c8bac9d695`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.