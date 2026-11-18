# Issue

**Title:** Updated transforms base

## Description

# Transform API Changes

This pull request introduces modifications to the `ParamsTransform` API. Now, transforms must be explicitly specified for each parameter, ensuring more precise control over their application. Additionally, the structure of the transforms (such as PyTrees or dictionaries) must match the structure of the parameters themselves. For further details, please refer to issue #343.

## Why These Changes?

The primary goal of these changes is to give *and* enforce manual control of reparameterizations to the User. Previously this was happening automatically, but suboptimal. 

## Example Usage

Below is an example demonstrating how to apply the updated API with custom transforms to various cell parameters in a `jaxley` model:

```python
import jaxley as jx
import jaxley.optimize.transforms as jt

# Define a model with compartments and branches
comp = jx.Compartment()
branch = jx.Branch(comp, nseg=2)
cell = jx.Cell(branch, parents=[-1, 0, 0])

# Specify which parameters to make trainable
cell.branch("all").make_trainable("radius")
cell.branch(2).make_trainable("radius")
cell.branch(1).make_trainable("length")
cell.branch(0).make_trainable("radius")
cell.branch("all").comp("all").make_trainable("axial_resistivity")
cell.make_trainable("capacitance")

# Retrieve the parameters
params = cell.get_parameters()

# Define a function to create appropriate transforms for each parameter
def create_transform(name):
    if name == "axial_resistivity":
        # Must be positive; apply Softplus and scale to match initialization
        return jt.ChainTransform([jt.SoftplusTransform(0), jt.AffineTransform(5000, 0)])
    elif name == "length":
        # Apply Softplus and affine transform for the 'length' parameter
        return jt.ChainTransform([jt.SoftplusTransform(0), jt.AffineTransform(10, 0)])
    else:
        # Default to a Softplus transform for other parameters
        return jt.SoftplusTransform(0)

# Apply the transforms to the parameters
transforms = [{k: create_transform(k) for k in param} for param in params]
tf = jt.ParamTransform(transforms)

# Obtain the unconstrained parameters
params_unconstrained = tf.inverse(params)
```

## Key Benefits

- **Explicit Transformations**: This change makes it clear which transformation is applied to each parameter, increasing transparency and control.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `26acd32dcbfa`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.