# Issue

**Title:** Clean up the iloc in the tests

## Description

Fix #202

I have cleaned up most of the `iloc` in the test but there are a couple of tests that still use `iloc`. Most of them are parameterised tests where different datasets demand different column labels.

```python
    @pytest.mark.parametrize(('data', 'size'), [(gmx_benzene_dHdl(), 4001),
                                                (gmx_benzene_u_nk(), 4001)])
    def test_subsampling(self, data, size):
        assert len(self.slicer(data, series=data.iloc[:, 0])) <= size
```

In this case, the first column is always chosen which have different column name for different datasets.

Another case is 
```
    def get_delta_f(self, est):
        ee = 0.0

        for i in range(len(est.d_delta_f_) - 1):
            ee += est.d_delta_f_.values[i][i+1]**2
        return est.delta_f_.iloc[0, -1], ee**0.5
```
Where depending on the dataset, this could be `est.delta_f_[0.0][1.0]` or `est.delta_f_.loc[(0.0,0.0)][(1.0,1.0)]`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `079a5b5e0aa4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.