# Issue

**Title:** Fix broken WHAMing if an InterfaceSet has more than 10 interfaces

## Description

This fixes an error that took a long time to track down and manifests itself with the same error as #920 (or with a wrong answer in more extreme cases).

**TLDR:** WHAMing  with more than 10 interfaces in an interfaceset was horribly broken, should now work 


 fixes the issue and adds a regression test
 updates the test file to use `pytest` instead (incremental update for #756) 

How this PR came to be:

This manifested itself as the same error as #920 (partial stacktrace):
```python
IndexError                                Traceback (most recent call last)
...

~/github_files/openpathsampling/openpathsampling/analysis/tis/crossing_probability.py in from_ensemble_histograms(self, hists)
    128         print(df.index)
    129         # TODO: remove WHAM-specific name here
--> 130         tcp = self.combiner.wham_bam_histogram(df).to_dict()
    131         return LookupFunction(tcp.keys(), tcp.values())

~/github_files/openpathsampling/openpathsampling/numerics/wham.py in wham_bam_histogram(self, input_df)
    503         """
    504         cleaned = self.prep_reverse_cumulative(input_df)
--> 505         self.check_cleaned_overlaps(cleaned)
    506         guess = self.guess_lnZ_crossing_probability(cleaned)
    507         sum_k_Hk_Q = self.sum_k_Hk_Q(cleaned)

~/github_files/openpathsampling/openpathsampling/numerics/wham.py in check_cleaned_overlaps(self, cleaned_df)
    477             prev_data = cleaned_df[prev_col]
    478 
--> 479             first_nonzero = col_data[col_data != 0.0].index[0]
    480             if not prev_data[first_nonzero] > 0.0:
    481                 # use not and > to account for NaNs

~/miniconda3/envs/tstis/lib/python3.7/site-packages/pandas/core/indexes/base.py in __getitem__(self, key)
   4099         if is_scalar(key):
   4100             key = com.cast_scalar_indexer(key, warn_float=True)
-> 4101             return getitem(key)
   4102 
   4103         if isinstance(key, slice):

IndexError: index 0 is out of bounds for axis 0 with size 0
```
From that issue we know that that happens if everything before the interface is set to `0` and everything after that interface is also `0`. So the first thing I did was setting cutoff to `0` to circumvent the same issue as in #920. 

This did not solve the error so I decided to add print functions to the `prep_reverse_cumulative` function of `numerics.wham.py` (sidenote: [`%autoreload` is great](https://switowski.com/blog/ipython-autoreload) and makes it so that I can debug this without waiting an hour to reload my data).

This is is the first block of  print(`df`), before any cleaning, notice anything peculiar about the column ordering?
 ```
            0         1        10       11        12        13        14  \
-2.6  1.000000  1.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-2.5  1.000000  1.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-2.4  1.000000  1.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-2.3  1.000000  1.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-2.2  1.000000  1.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-2.1  1.000000  1.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-2.0  1.000000  1.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-1.9  1.000000  1.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-1.8  1.000000  1.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-1.7  1.000000  1.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-1.6  1.000000  1.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-1.5  1.000000  1.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-1.4  1.000000  1.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-1.3  1.000000  1.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-1.2  1.000000  1.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-1.1  1.000000  1.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-1.0  1.000000  1.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-0.9  0.739261  1.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-0.8  0.548452  0.826174  1.000000  1.00000  1.000000  1.000000  1.000000   
-0.7  0.221778  0.286713  1.000000  1.00000  1.000000  1.000000  1.000000   
-0.6  0.038961  0.022977  1.000000  1.00000  1.000000  1.000000  1.000000   
-0.5  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-0.4  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-0.3  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-0.2  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
-0.1  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 0.0  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 0.1  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 0.2  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 0.3  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 0.4  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 0.5  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 0.6  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 0.7  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 0.8  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 0.9  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 1.0  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 1.1  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 1.2  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 1.3  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 1.4  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 1.5  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 1.6  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 1.7  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 1.8  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 1.9  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 2.0  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 2.1  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 2.2  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 2.3  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 2.4  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 2.5  0.000000  0.000000  1.000000  1.00000  1.000000  1.000000  1.000000   
 2.6  0.000000  0.000000  0.325674  0.52048  0.340659  0.416583  0.643357   
```
Now here is where things go horribly wrong: line `137-142` of `wham.py` are:
```python
          if self.interfaces is not None:                                         
              # use the interfaces values to set anything before that value to    
              # zero                                                              
              if type(self.interfaces) is not pd.Series:                          
                  self.interfaces = pd.Series(data=self.interfaces,                  
                                              index=df.columns)  
```
Because this is how `self.interfaces` looks before converting it to a series:
```python
[-1.0, -0.9, -0.8, -0.7000000000000001, -0.6000000000000001, -0.5000000000000001, -0.40000000000000013, -0.30000000000000016, -0.20000000000000018, -0.1000000000000002, -2.220446049250313e-16, 0.09999999999999964, 0.19999999999999973, 0.2999999999999998, 0.3999999999999997, 0.49999999999999956, 0.5999999999999996, 0.6999999999999997, 0.7999999999999996, 0.8999999999999995, 0.9999999999999996]
```
and this is after
```python
0    -1.000000e+00
1    -9.000000e-01
10   -8.000000e-01
11   -7.000000e-01
12   -6.000000e-01
13   -5.000000e-01
14   -4.000000e-01
15   -3.000000e-01
16   -2.000000e-01
17   -1.000000e-01
18   -2.220446e-16
19    1.000000e-01
2     2.000000e-01
20    3.000000e-01
3     4.000000e-01
4     5.000000e-01
5     6.000000e-01
6     7.000000e-01
7     8.000000e-01
8     9.000000e-01
9     1.000000e+00
``` 

So this now maps the 3rd interface to the data of the 10th, but even worse is the 13th interface is mapped to the data of the second! The data from my 2nd interface does not have any data that is past the 13th, so this results in a column of `0`s after cleaning (everything up to the 13th interface is set to 0).

Now, this PR does not actually alter `wham.py` because the real issue is the sorting of the columns of the dataframe.
This is done at line `124-127` of `openpathsampling/analysis/tis/crossing_probability.py`
```python
        df = paths.numerics.histograms_to_pandas_dataframe(                     
            input_hists,                                                        
            fcn="reverse_cumulative"                                               
        ).sort_index(axis=1)  
```
So this sorts axis=1, and what we see is that th columns are sorted, but as if they are `str`s, as those are sorted character by character, which is how you end up with `0, 1, 10, 11, ..., 19, 2, 20, 3, ...` . Now where do these strings come from? 

That is what is actually fixed in this PR
If we look at the called function (`histograms_to_pandas_dataframe`) on line 432-433 the follwing code exists:
```python
      if hist.name is None:                                                   
              hist.name = str(hists.index(hist))     
```
and these `str` indices end up in the dataframe. Hence this PR to convert them to an `int` instead.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1279f237d60b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.