# Issue

**Title:** add no_copy option to NoModifier and Deprecate 'subset_mask'

## Description

This is a cherry picked version of  from #1075 in order to seperate out the comments

Copies of relevant comments:
> @dwhswenson wrote:
> More thoughts coming later, but here's a really quick one:
>
>    even with NoModification I am pretty sure new == old but not new is old (only a copy is returned).
>
>Looking at the code, you're correct. In usage here, I think we want the default modifier (for one-way shooting, e.g.) to return `new`  such that new is old. My suggestion (pretty easy change to the current code):
>```
>class NoModification(SnapshotModifier):
>   def __init__(self, as_copy=True):
>     # masking change to only certain atoms is nonsense for no modification
>       super(NoModification, self).__init__(subset_mask=None)
>       self.as_copy = as_copy
>
>    def __call__(self, snapshot):
>        return snapshot.copy() if self.as_copy else snapshot
>```
> Then use NoModification(as_copy=False) as the default in the changes here.

> @sroet wrote:
> That was way more nasty than expected, as we actually test for subset_mask on NoModification (as a proxy for subclassing SnapshotModifier) Also, apparently old.copy() != old for toy snapshots, I did not really investigate this, but I am afraid we are losing precision on the copy when seeing all the assert_almost_equal in the tests for this modifier...

> @dwhswenson wrote:
> > I can cherry pick over that commit into a separate PR if that is preferred
> 
> Yeah, if it's going to be a headache, let's move it into a separate PR to keep discussion a little cleaner.
> 
> > That was way more nasty than expected, as we actually test for subset_mask on NoModification (as a proxy for subclassing `SnapshotModifier`)
> 
> That part is easily fixed by subclassing in tests. I was lazy before; sorry! man_shrugging
> 
> > Also, apparently `old.copy() != old` for toy snapshots, I did not really investigate this, but I am afraid we are losing precision on the copy when seeing all the `assert_almost_equal` in the tests for this modifier...
> 
> I'm not surprised by this. There's enough bouncing between C and Python representations that I wouldn't be surprised if these are only `almost_equal`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d9832f85792d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.