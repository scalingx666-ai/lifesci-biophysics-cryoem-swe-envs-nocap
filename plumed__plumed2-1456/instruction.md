# Issue

**Title:** Fix a deadlock the GROMACS 2026 patch introduces in -multidir runs

## Description

##### Description

I wrote the GROMACS 2026 patch in #1439. Reviewing it after it merged, I found it introduces a
deadlock in multi-simulation runs — including runs that never pass `-plumed`. This fixes that.

**Where this lives:** on the `v2.10` and `master` branches only. It is **not in any released PLUMED** —
`v2.10.1` was tagged 2026-07-01 and #1439 merged 2026-08-27, so `patches/gromacs-2026.0.diff` does not
exist in any release. It would ship in the next v2.10.x, so this is a fix-before-release.

### The defect

`plumedMDModule.cpp` subscribes to the multi-simulation record unconditionally:

```cpp
notifier->simulationSetupNotifier_.subscribe([this](const gmx_multisim_t* ms)
                                             { this->options_.setMultisim(ms); });
```

`md.cpp` infers lockstep checkpointing from *whether anyone subscribed*, not from whether PLUMED is
active:

```cpp
bool simulationsShareState =
        (ms_ != nullptr)
        && mdModulesNotifiers_.simulationSetupNotifier_.haveSubscribers<const gmx_multisim_t*>();
```

So a patched GROMACS enables inter-simulation signalling and the checkpoint-rename barrier across
`ms->mainRanksComm_` for **every** `-multidir` run, with or without `-plumed`. When the simulations run
different numbers of steps they never all reach the barrier and the run hangs. This is why the
equivalent change to GROMACS's own module ([GROMACS !6159](https://gitlab.com/gromacs/gromacs/-/merge_requests/6159))
was not merged.

**The patch introduces this; it is not inherited.** Stock GROMACS 2026 has no subscriber for that type
anywhere:

- `plumedMDModule.cpp.preplumed`: `grep -c gmx_multisim_t` → **0**
- current `release-2026`: **0**
- `git grep "subscribe.*gmx_multisim_t" -- src/` → **no matches**

The last is a whole-tree claim, checkable in one command: no module anywhere in stock GROMACS 2026
subscribes to that type. `haveSubscribers<const gmx_multisim_t*>()` therefore cannot be true there, and
the patch is the sole introducer.

**Reach: every released GROMACS 2026.** The stored `plumedMDModule.cpp` baseline is byte-identical to
that file at **v2026.0, v2026.1, v2026.2 and v2026.3** (all four checked), so this hunk applies cleanly
to every released 2026 and the subscription lands on all of them.

### The fix — subscribe lazily

Subscribe to `const gmx_multisim_t*` only once `PlumedInputFilename` has shown PLUMED is in use. Two
properties were checked rather than assumed:

- **Ordering.** `PlumedInputFilename` is notified at `runner.cpp:1141` and `gmx_multisim_t*` at
  `runner.cpp:1801` — same function, sequential. The late subscription is in place in time. Had the
  order been reversed, this fix would have silently reintroduced the unset-`multi_sim_comm` bug it
  exists to avoid.
- **Safety of subscribing during a dispatch.** `MDModulesNotifier` is a per-type template chain; each
  `CallParameter` level owns its own `callBackFunctions_` vector (`mdmodulesnotifier.h:148`), `notify`
  iterates only that type's vector (`:112`) and `subscribe` appends only to that type's (`:125`).
  Subscribing to `gmx_multisim_t*` while dispatching `PlumedInputFilename` touches a different vector,
  so no iterator is invalidated.

### Evidence status

Verified by inspection; **not reproduced on the `plumed patch` route.** The subscription is
character-identical — for the type `haveSubscribers` actually counts — to the one in GROMACS !6159,
whose hang was reproduced in the equivalent native (`GMX_USE_PLUMED`) build. Since `plumed patch` edits
GROMACS's *own* PLUMED module rather than a parallel implementation, the two are the same code path. It
has not been run against a patched tree.

### Not in this PR

A separate, unrelated issue exists in the same patch directory: the stored `plumedforceprovider.cpp`
baseline predates GROMACS !6133, so the patch does not apply to `release-2026` tip. That affects **no
released GROMACS** (v2026.0–v2026.3 all apply cleanly) and is deliberately left out of this PR.

##### Target release

I would like my code to appear in release __v2.10__

##### Type of contribution

- [X] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [ ] changes to a module not authored by you
- [ ] new module contribution or edit of a module authored by you

##### Copyright

- [X] I agree to transfer the copyright of the code I have written to the PLUMED developers or to the author of the code I am modifying.

##### Tests

- [ ] I added a new regtest or modified an existing regtest to validate my changes.
- [ ] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

<!-- Both test boxes left unticked deliberately: this changes a stored patch file, which the regtest
suite does not exercise (it would need a patched GROMACS build). I am happy to add a regtest if you
would like one, and will tick the second box once CI has run on this PR. -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `458397b55ff3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.