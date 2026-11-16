# Issue

**Title:** Evaluation of preferential orientation

## Description

This PR introduces a method to analyze the kind and degree of preferential orientation across multiple experiments. The method can be accessed in two different ways. Firstly, it is accessible as a standalone tool via `cctbx.xfel.preference`, providing a quick method to analyze small amounts of data. Additionally, preferential orientation can be analyzed via a dedicated `cctbx.xfel.merge` worker called `preference`, which takes no phil parameters and can be easily introduced before merging. Both methods can be run using MPI.

The degree of preferential orientation, i.e. the "preference", is evaluated by fitting a Watson Distribution to the list of normalized lattice directions across all experiments. The [Watson distribution](https://doi.org/10.2307%2F3213566) is a conceptually simple generalization of the Gaussian distribution for a sphere (though I really struggled to find good resources about it). The concentration of unit vectors is described using two parameters – vector μ and scalar κ:
- In a distribution described by κ << 0, vectors focus around equator away from ±μ;
- In a distribution described by κ ≈ 0, vectors are distributed ~uniformly on the sphere. 
- In a distribution described by κ >> 0, vectors focus around ±μ;

![Screen Shot 2024-02-08 at 5 30 43 PM](https://github.com/cctbx/cctbx_project/assets/24624030/4cc89f06-7840-4316-b07f-24c507022c6c)
_Figure: Red vectors following Watson distribution around black μ=[0,0,1] with κ = -10, ~0, and 10, respectively._

With that in mind, the general algorithm for evaluating preference is as follows:
- Read experiments from `input.glob` or the previous worker;
- Choose the space group:
  - EITHER Read from `input.space_group` (standalone only)...
  - OR Automatically find the most common space group;
- Generate a list of small symmetrically unique zone axes indices {h,k,l} to be investigated;
  - Each unique zone axis describes a unique possible type of crystal face;
  - In particular, zone axes {1,0,0}, {0,1,0}, and {0,0,1} include vectors **a***, **b***, and **c***.
- For each unique zone axis:
  - Generate a list of directions of the zone axis across all experiments;
  - Apply all Laue Class symmetry operations to vectors to avoid selection bias (`input.symmetrize`);
  - Fit Watson distribution to the list of directions
- Write out a table of Watson κ and μ parameters
- Plot the distribution of vectors

The most important product is the listing of κ and μ values for zone axes, in particular, the value of κ in the first row.
```
----------------------------------------------------
Direction  kappa   mu                      NLL
----------------------------------------------------
{0,0,1}    -1.208  [-0.708,+0.706,-0.027]  -7.41E+03
{1,0,0}    +0.593  [+0.711,-0.703,+0.011]  -2.29E+03
{2,-1,0}   +0.581  [+0.710,-0.704,+0.012]  -2.21E+03
{0,1,0}    -0.623  [-0.709,-0.701,+0.083]  -2.17E+03
{0,1,-2}   -0.609  [-0.709,+0.705,-0.020]  -2.08E+03
{1,-1,0}   +0.555  [+0.709,-0.705,+0.017]  -2.00E+03
{2,0,-1}   +0.539  [-0.711,+0.703,-0.010]  -1.89E+03
{2,-1,-1}  +0.531  [+0.711,-0.704,+0.012]  -1.83E+03
{2,-2,-1}  +0.513  [+0.710,-0.704,+0.016]  -1.71E+03
{1,-2,0}   +0.500  [+0.708,-0.706,+0.029]  -1.62E+03
{0,2,-1}   -0.429  [-0.710,-0.699,+0.079]  -1.06E+03
{1,-2,-1}  +0.407  [+0.708,-0.706,+0.029]  -1.06E+03
{1,-1,-1}  +0.401  [+0.710,-0.704,+0.014]  -1.03E+03
{2,-1,-2}  +0.398  [-0.711,+0.703,-0.009]  -1.02E+03
{1,0,-1}   +0.397  [+0.712,-0.702,+0.007]  -1.01E+03
{1,-2,-2}  +0.181  [+0.707,-0.706,+0.032]  -2.05E+02
{0,1,-1}   +0.144  [+0.050,+0.026,+0.998]  -1.29E+02
{1,0,-2}   -0.134  [+0.072,+0.018,+0.997]  -1.08E+02
{1,-1,-2}  -0.090  [+0.037,+0.027,+0.999]  -4.95E+01
----------------------------------------------------
```
This is how the table might look for Laue class mmm. Here, κ < -1 suggests a strong preference for zone axes {001} = (001) and (00-1) to point away from direction [-1,1,0] in the laboratory space due to the use of tilted Kapton tape. Other zone axes also feature κ < -0.1 or κ > +0.1 strongly suggesting, that preferential orientation might be present. For decently large datasets of ~> 10000 experiments, uniform distributions should yield κ below 0.1.

The distribution of individual zone axes is additionally plotted; in standalone, the default `plot.style=hammer`, which shows one heatmap of vector distribution on a unit sphere for each zone axis:

![Screen Shot 2024-02-08 at 6 06 42 PM](https://github.com/cctbx/cctbx_project/assets/24624030/2f46402e-da39-42e0-b994-a73812908e6d)

Since the worker will usually run in a headless environment, a single small 2d heatmap of vector density across azimuth/polar coordinates will be printed in the main log using ASCII characters instead:

```
Ascii distribution heat plot for direction {0,0,1}:
┌────────────────────Z───────────────────┐
│▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▓▓▓▓▒▒▒▒▒▒▒▒▒▒▒▒│
│▒▒▒▒▒▒▒▒▒▒▒▒░░░░░░░░▓▓▓▓▓▓▓▓▒▒▒▒░░░░▒▒▒▒│
│▒▒▒▒▓▓▓▓░░░░░░░░░░░░▓▓▓▓▓▓▓▓▒▒▒▒░░░░░░░░│
│▒▒▒▒▓▓▓▓░░░░░░░░░░░░▒▒▒▒▓▓▓▓░░░░    ░░░░│
│▒▒▒▒████░░░░        ▒▒▒▒████░░░░        │
X▒▒▒▒████░Y░░        X▒▒▒████░░Y░        X
│▒▒▒▒▓▓▓▓░░░░    ░░░░▒▒▒▒▓▓▓▓░░░░░░░░░░░░│
│▓▓▓▓▓▓▓▓▒▒▒▒░░░░░░░░▒▒▒▒▓▓▓▓░░░░░░░░░░░░│
│▓▓▓▓▓▓▓▓▒▒▒▒░░░░▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒░░░░░░░░│
│▒▒▒▒▓▓▓▓▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒│
└────────────────────Z───────────────────┘

```
![Screen Shot 2024-02-08 at 6 09 04 PM](https://github.com/cctbx/cctbx_project/assets/24624030/aad01ce8-f126-4a3c-81f8-faaef243d6e7)

These maps can be also generated for all zone axes in the standalone using `plot.style=ascii`. Since they only use ASCII characters, there shouldn't be any problems writing them or storing code in cctbx.

To sum up, this is not the best way to analyze preferential orientation, but in my opinion, it is simple and intuitive. The better way would be to analyze the distribution of entire [**a**,**b**,**c**] matrices instead of individual vectors, but I don't know how to do it. Also, the numeric value of κ will be 0 in cubic systems, even if there is some preferential orientation, due to its inherent symmetry. In this case, one would need to look at the plots to find it. This isn't a big issue since preferential orientation should not cause any significant problems for cubic crystals.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cf9adb532103`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.