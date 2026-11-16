# Issue

**Title:** Eliminate allocations from ManagedArray::operator[] and Histogram::operator() for a ~2x performance jump in most order parameters

## Description

<!-- Provide a general summary of your changes in the Title above. -->

## Description

Alternative to #1429, my goal is to further speed up the managedarray accessor api. This PR is actually even faster, as I found an issue with inlining the bound check that I did not in the previous PR.

Also, sorry for a new, separate PR - I forgot I had already opened one for this change. This implementation is clearer, faster, and fixes the same issue in Histogram.h, so I think its better to continue the review from here.

## Motivation and Context

```
# postprocessed from python benchmarker.py run -o bench.jsonbenchmarks/reports/bench.json $BASE $PR
freud.PMFT.PMFTR12               100 |      0.3 ->     0.2 ms   1.3x | c1      0.8 ->     0.4 ms   2.4x
freud.PMFT.PMFTR12               500 |      3.8 ->     2.0 ms   1.9x | c1     17.9 ->     6.4 ms   2.8x
freud.PMFT.PMFTR12              1000 |     13.6 ->     5.3 ms   2.6x | c1     69.3 ->    23.6 ms   2.9x
freud.PMFT.PMFTR12              2000 |     49.7 ->    19.3 ms   2.6x | c1    273.3 ->    93.1 ms   2.9x
freud.PMFT.PMFTXY                100 |      0.3 ->     0.3 ms   1.2x | c1      0.6 ->     0.3 ms   2.2x
freud.PMFT.PMFTXY                500 |      2.3 ->     1.2 ms   1.9x | c1     11.0 ->     3.6 ms   3.1x
freud.PMFT.PMFTXY               1000 |      7.9 ->     2.8 ms   2.9x | c1     42.1 ->    12.2 ms   3.5x
freud.PMFT.PMFTXY               2000 |     31.8 ->    12.1 ms   2.6x | c1    163.2 ->    43.4 ms   3.8x
freud.PMFT.PMFTXYT               100 |      0.8 ->     0.4 ms   1.7x | c1      1.0 ->     0.5 ms   2.0x
freud.PMFT.PMFTXYT               500 |      3.8 ->     2.0 ms   2.0x | c1     17.8 ->     6.8 ms   2.6x
freud.PMFT.PMFTXYT              1000 |     13.3 ->     5.5 ms   2.4x | c1     68.7 ->    24.3 ms   2.8x
freud.PMFT.PMFTXYT              2000 |     49.5 ->    18.6 ms   2.7x | c1    268.0 ->    91.4 ms   2.9x
freud.PMFT.PMFTXYZ               100 |     20.5 ->    13.7 ms   1.5x | c1     14.1 ->    10.3 ms   1.4x
freud.PMFT.PMFTXYZ               500 |     22.4 ->    14.8 ms   1.5x | c1     28.4 ->    16.1 ms   1.8x
freud.PMFT.PMFTXYZ              1000 |     33.5 ->    17.5 ms   1.9x | c1     71.7 ->    31.6 ms   2.3x
freud.PMFT.PMFTXYZ              2000 |     60.0 ->    28.1 ms   2.1x | c1    242.9 ->    90.0 ms   2.7x
freud.cluster.Cluster           1000 |      0.3 ->     0.3 ms   0.9x | c1      0.8 ->     0.8 ms   1.1x
freud.cluster.Cluster           5000 |      2.4 ->     2.6 ms   0.9x | c1      9.0 ->     8.7 ms   1.0x
freud.cluster.Cluster          10000 |      6.2 ->     6.4 ms   1.0x | c1     28.4 ->    26.7 ms   1.1x
freud.density.CorrelationFunctio1000 |     12.5 ->    10.9 ms   1.2x | c1     41.3 ->    41.2 ms   1.0x
freud.density.CorrelationFuncti10000 |   1005.3 ->   871.1 ms   1.2x | c1   3793.9 ->  3760.4 ms   1.0x
freud.density.GaussianDensity   1000 |      1.4 ->     0.5 ms   2.5x | c1      6.1 ->     1.8 ms   3.4x
freud.density.GaussianDensity  10000 |     11.7 ->     3.9 ms   3.0x | c1     59.7 ->    17.2 ms   3.5x
freud.density.LocalDensity      1000 |      1.6 ->     1.8 ms   0.9x | c1      7.3 ->     6.8 ms   1.1x
freud.density.LocalDensity     10000 |     16.4 ->    17.8 ms   0.9x | c1     80.4 ->    73.9 ms   1.1x
freud.density.RDF               1000 |      6.2 ->     3.5 ms   1.8x | c1     21.8 ->    11.2 ms   1.9x
freud.density.RDF              10000 |    398.7 ->   201.5 ms   2.0x | c1   1683.2 ->   735.2 ms   2.3x
freud.environment.BondOrder        4 |      0.2 ->     0.2 ms   1.1x | c1      0.7 ->     0.4 ms   1.8x
freud.environment.BondOrder        8 |      1.3 ->     1.0 ms   1.3x | c1      5.6 ->     3.2 ms   1.8x
freud.environment.BondOrder       16 |     10.0 ->     6.9 ms   1.5x | c1     45.6 ->    25.8 ms   1.8x
freud.locality.AABBQuery        1000 |      0.0 ->     0.0 ms   1.0x | c1      0.0 ->     0.0 ms   0.9x
freud.locality.AABBQuery       10000 |      0.9 ->     1.0 ms   1.0x | c1      0.9 ->     1.0 ms   1.0x
freud.locality.CellQuery        1000 |      0.6 ->     0.2 ms   2.4x | c1      0.9 ->     0.5 ms   1.9x
freud.locality.CellQuery       10000 |     22.9 ->    11.0 ms   2.1x | c1     72.6 ->    34.0 ms   2.1x
freud.locality.LinkCell         1000 |      0.0 ->     0.0 ms   2.0x | c1      0.0 ->     0.0 ms   1.8x
freud.locality.LinkCell        10000 |      0.1 ->     0.1 ms   2.3x | c1      0.1 ->     0.1 ms   1.8x
freud.locality.PeriodicBuffer   1000 |      0.2 ->     0.2 ms   1.0x | c1      0.2 ->     0.2 ms   1.0x
freud.locality.PeriodicBuffer   5000 |      1.0 ->     1.0 ms   1.0x | c1      1.2 ->     1.0 ms   1.3x
freud.locality.PeriodicBuffer  10000 |      2.0 ->     2.1 ms   0.9x | c1      2.1 ->     2.0 ms   1.0x
freud.msd.MSD                    100 |      0.1 ->     0.1 ms   1.3x | c1      0.1 ->     0.1 ms   1.0x
freud.msd.MSD                    500 |      0.2 ->     0.2 ms   1.0x | c1      0.2 ->     0.2 ms   1.0x
freud.msd.MSD                   1000 |      0.4 ->     0.4 ms   1.0x | c1      0.4 ->     0.4 ms   1.1x
freud.msd.MSD                   5000 |      2.2 ->     1.9 ms   1.1x | c1      1.8 ->     1.8 ms   1.0x
freud.order.Cubatic             1000 |      0.3 ->     0.3 ms   0.9x | c1      0.8 ->     0.8 ms   1.0x
freud.order.Cubatic             5000 |      0.6 ->     0.6 ms   1.0x | c1      2.1 ->     2.0 ms   1.1x
freud.order.Cubatic            10000 |      1.0 ->     1.4 ms   0.7x | c1      3.8 ->     3.5 ms   1.1x
freud.order.Hexatic              100 |      0.1 ->     0.1 ms   1.6x | c1      0.2 ->     0.2 ms   0.9x
freud.order.Hexatic              500 |      1.3 ->     1.3 ms   1.0x | c1      5.4 ->     5.4 ms   1.0x
freud.order.Hexatic             1000 |      3.9 ->     4.5 ms   0.9x | c1     19.2 ->    19.4 ms   1.0x
freud.order.Hexatic             5000 |     73.2 ->    80.3 ms   0.9x | c1    370.6 ->   374.7 ms   1.0x
freud.order.Hexatic            10000 |    248.2 ->   281.2 ms   0.9x | c1   1346.4 ->  1353.1 ms   1.0x
freud.order.RotationalAutocorrel1000 |      0.1 ->     0.0 ms   1.4x | c1      0.1 ->     0.1 ms   1.7x
freud.order.RotationalAutocorrel5000 |      0.2 ->     0.1 ms   1.5x | c1      0.7 ->     0.4 ms   1.8x
freud.order.RotationalAutocorre10000 |      0.3 ->     0.3 ms   1.3x | c1      1.4 ->     0.8 ms   1.8x
freud.order.SolidLiquid          100 |      0.4 ->     0.4 ms   1.0x | c1      0.4 ->     0.2 ms   1.7x
freud.order.SolidLiquid          500 |      6.8 ->     6.5 ms   1.0x | c1      7.3 ->     4.3 ms   1.7x
freud.order.SolidLiquid         1000 |     25.3 ->    24.1 ms   1.0x | c1     25.2 ->    15.9 ms   1.6x
freud.order.SolidLiquid         5000 |    619.3 ->   549.3 ms   1.1x | c1    562.1 ->   367.3 ms   1.5x
freud.order.SteinhardtQl         100 |      0.1 ->     0.1 ms   1.0x | c1      0.1 ->     0.1 ms   1.2x
freud.order.SteinhardtQl         500 |      0.5 ->     0.5 ms   1.1x | c1      2.2 ->     1.7 ms   1.3x
freud.order.SteinhardtQl        1000 |      1.6 ->     1.6 ms   1.0x | c1      6.9 ->     5.9 ms   1.2x
freud.order.SteinhardtQl        5000 |     27.4 ->    25.2 ms   1.1x | c1    145.4 ->   122.8 ms   1.2x
freud.order.SteinhardtWl         100 |      0.1 ->     0.1 ms   1.1x | c1      0.2 ->     0.2 ms   1.2x
freud.order.SteinhardtWl         500 |      0.8 ->     0.5 ms   1.6x | c1      2.1 ->     1.9 ms   1.1x
freud.order.SteinhardtWl        1000 |      2.4 ->     1.5 ms   1.6x | c1      7.2 ->     6.4 ms   1.1x
freud.order.SteinhardtWl        5000 |     36.2 ->    26.2 ms   1.4x | c1    149.5 ->   124.3 ms   1.2x

```

## How Has This Been Tested?
<!-- Please describe in detail how you tested your changes. -->
<!-- Include details of your testing environment, and the tests you ran to
     see how your changes affect other areas of the code, etc. -->

## Checklist:
- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/freud/blob/main/CONTRIBUTING.md).
- [x] I agree with the terms of the [**Freud Contributor Agreement**](https://github.com/glotzerlab/freud/blob/main/ContributorAgreement.md).
- [x] My name is on the [list of contributors](https://github.com/glotzerlab/freud/blob/main/doc/source/reference/credits.rst) in the pull request source branch.
- [x] I have updated the [Change log](https://github.com/glotzerlab/freud/blob/main/ChangeLog.md).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `00256e53e674`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.