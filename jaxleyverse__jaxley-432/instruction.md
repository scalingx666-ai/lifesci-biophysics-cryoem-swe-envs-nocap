# Issue

**Title:** Plotting of comp morphology

## Description

I got side-tracked while working on a different PR and implemented plotting of the actual compartment structure:

The compartments are rendered in 3D and then the correct projection of the 3d mesh is plotted on whatever plane. 
```
fig, ax = plt.subplots(2, 3, figsize=(10, 5))

for i, orientation in enumerate([np.array([1,1,2]), np.array([0,1,0.1]), np.array([0,0.1,1])]):
    plot_cylinder_projection(orientation, length=5, radius=1, center=np.array([0,0,0]), dims=[0,1], ax=ax[0,i])
    plot_cylinder_projection(orientation, length=5, radius=1, center=np.array([0,0,0]), dims=[1,2], ax=ax[1,i])
    ax[0,i].set_title(f"Orientation: {orientation}")
plt.show()
```
![image](https://github.com/user-attachments/assets/2581b65b-0442-4131-bc92-e366f49197c8)


Hence the compartment radii and lengths are correctly reflected in the plot.
![image](https://github.com/user-attachments/assets/47ad6a53-0fe9-469d-8e29-fb4335eadaf1)

However, plotting an imported morphology looked weird for some reason.
![image](https://github.com/user-attachments/assets/3de51f5c-0dfc-451a-b54e-3adc1492c7aa)

While looking into this, I noticed two things:
Since the cylindrical compartments are straight, they run from one compartment edge to the other on a straight line. However, they model a potentially very zigzaggy branch/path structure. This means:
a) Assuming the comp center is at loc=0.5 along the path (currently how we do it), can put the comp center off axis. 
b) Setting the comp length to the path length can result in comps which appear longer than the distance between their two endpoints.

This can be illustrated by adding a bit of noisy to the xyz coordinates of the example above.
![image](https://github.com/user-attachments/assets/2b30b465-dce3-4d3f-8984-c097ed02b077)

Changing the compartment centers to be the center of the cylinder, already produces much better looking plots.
![image](https://github.com/user-attachments/assets/4dde388f-80fa-464f-af0c-f06e2862b0f5)

While I dont think changing b) makes sense (apart from for visualization maybe) I think changing a) might be a good idea.

Lemme know your thoughts.

_PR is missing documentation, some cleanup and handling of single point cases._

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7648bbea1621`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.