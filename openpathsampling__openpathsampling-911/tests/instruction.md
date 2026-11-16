# Issue

**Title:** "Hooks" for PathSampling

## Description

This builds on #755 and should only be merged after that.

It adds hook support to the `PathSampling`, the `PathSamplingOutputHook` and the `LiveVisualizerHook` including tests.

Please let me know if htere is anything to improve!
I also have some open questions/TODOs, in particular:
1. if a user sets any attribute that controls the behaviour of the hook on the `PathSimulator` object it is only read once at the start of the simulation and only if the hooks value for that attribute is `None` (see e.g. `output_stream`, `save_frequency` and `storage`). This is potentially confusing, as I would expect to be able to set these attributes on the `PathSimulator` to influence the behaviour of the currently attached hooks. I am mot really sure how to adress this. Potential solutions include removing these attributes from the `PathSimulator` and only set them on hook creation. Another option is to make them properties to either (re)set the respective attributes on the hooks (or at least warn the user that it could not work as intended) if he/she sets the attributes on the `Pathsimulator`

2. Previously the LiveVisualizer was called before the MCstep but visualizing the previous step. I moved the LiveVisualization to after step. This enables visualization of the step that just finished and avoids that the beginning of the next step depends on the previous one. But it also means that visualization only works if the serial OutputHook has `allow_refresh=False`, otherwise the plot will get refreshed away.

3. The `step_info` passed to the `after_step` hooks from the `PathSampling` is specifically tailored for the `PathSamplingOutputHook`, i.e. it only takes the variables the OutputHook needs (current step, total steps, total elapsed time). @dwhswenson any ideas what else we could want/need in there in the future?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b6da5ddf1498`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.