# Issue

**Title:** iotbx: supplied-TLS test checks the TLS step directly instead of through full sharpening

## Description

## Problem
`iotbx/regression/tst_map_model_manager_supplied_tls.py` ran as 25 jobs
taking about 540 CPU-s, close to half of the iotbx suite. Most cases ran a
complete local anisotropic `model_sharpen` (FSC in 8 boxes, a map-symmetry
search, scaling in every direction) only to obtain one original grid
entry. Everything they assert is what the TLS step
(`_analyze_aniso_replace_with_supplied`) does with that entry and with the
model points.

## Changes
Only the test and `iotbx/run_tests.py` change; library code is untouched.
Every case keeps its name and the intent of its docstring.

- **Direct** (two_tls_groups, overlap_nearest, reversed_order,
  sampling_count, empty_mask_group, tie_first_listed,
  no_model_first_listed, multi_grid_ownership, tls_in_other_frame,
  all_tls_entries_updated, and parts of half_map_with_model and
  tls_reused): the TLS step is called directly on synthetic entries placed
  where they test a rule. Those places are the grid entry the real run
  produces (inside both masks of split A), each group's centre, and a
  corner outside every mask; multi_grid_ownership uses a 2.5 A lattice
  over the model (560 entries). The existing checks (`get_ownership`,
  `check_entry_u`, `compare_orders`, the sampling rule) run unchanged on
  what the TLS step returns. New check: an entry inside no mask keeps its
  U.
- **Stubbed** (options_passed_on, tls_reused, global_local_global_model,
  global_local_global_half_map, part of half_map_with_model): local
  anisotropic sharpening (`_run_group_of_anisotropic_sharpen`) is replaced
  by a stub. The stub records what would reach the TLS step (options, T
  and L of each group, atoms of each group in the model it uses) and
  passes the maps through unchanged. The overall passes of
  global-local-global still run.
- **Complete runs:** the external_* cases and no_tls_unchanged as before.
  other_model_id also stays complete, because only a complete run catches
  a `model_id` dropped between the sharpening driver and the TLS step (the
  bug fixed in d7f4b2d998). It now also checks that the scale factors are
  updated after the TLS step, which options_passed_on used to check.
- `run_tests.py`: 25 entries become 6. One job runs the 18 fast cases
  through the new argument `fast` (their names as a list would make too
  long a directory name under `run_in_unique_dirs`); five run the
  complete cases, each under 30 s.

Timing on one machine, with the same library code, 6 jobs at a time:

| | jobs | CPU-s | longest job |
|---|---|---|---|
| before | 25 | 540 | 70 s |
| after | 6 | 115 | 27 s |

Coverage that is thinner than before, by design:

- The global-local-global cases no longer run the TLS step itself.
- No case runs the real sharpening driver with TLS through
  `half_map_sharpen`; external_tls still does through `external_sharpen`.
- tls_in_other_frame calls the TLS step on a boxed manager directly
  rather than through the working copies made by `model_sharpen`.

The fast job takes 12 s, above the 10 s aimed for. 8 s of it are the two
global-local-global cases, whose overall sharpening passes are not
stubbed.

## Tests
- All 25 cases pass, both run alone and together.
  `libtbx.run_tests_parallel module=iotbx nproc=8 run_in_unique_dirs=True`
  on this branch: 194 tests, 0 failures. Platform: macOS arm64.
- Mutation check: each of 21 mutants of `iotbx/map_model_manager.py`
  re-introduces a bug that a case was written for. 17 revert parts of the
  fixes in 12fc98df99, 2c84ae8767, d7f4b2d998 and 1ac7769a32; 4 came up
  in review. Every mutant is caught by the original file and by the new
  one:

<details>
<summary>Mutants and the cases that catch them</summary>

| mutant | caught by the original | caught by the new file |
|---|---|---|
| grid entry in several masks goes to the group processed last | 9 cases | 10 cases, incl. overlap_nearest, multi_grid_ownership |
| every group uses the first group's TLS | 12 | 11, incl. two_tls_groups |
| model points sampled with the growing entry count | sampling_count, reversed_order, half_map_with_model | same |
| model points re-tested against later groups' masks | 10 | 8, incl. two_tls_groups, reversed_order |
| group without grid entries uses the previous group's TLS for its model points | empty_mask_group, reversed_order | empty_mask_group |
| exact tie, or no model, goes to the group listed last | tie_first_listed, no_model_first_listed | same |
| TLS frame shift with the wrong sign | tls_in_other_frame | same |
| model points not moved to the TLS frame | tls_in_other_frame | same |
| overall anisotropy not removed from the TLS U | 12 | 10 |
| averaged overall_scale not stored | all_tls_entries_updated + 3 averaging cases | same + other_model_id |
| get_scale_as_aniso_u not passed on | options_passed_on, all_tls_entries_updated | options_passed_on, other_model_id |
| get_tls_from_u not passed on by model_sharpen | options_passed_on | same |
| converting tlso_group_info twice fails | tls_reused, both global_local_global, tls_in_other_frame | same |
| half_map_sharpen converts after copying for global-local-global | global_local_global_half_map | same |
| model_sharpen does not pass its model_id | other_model_id | same |
| driver does not pass model_id to the TLS step | other_model_id | same |
| half-map/external working manager drops the model | external_tls, half_map_with_model, global_local_global_half_map | same |
| scale factors not updated after the TLS step | options_passed_on, all_tls_entries_updated | other_model_id |
| TLS step without its default model id | half_map_with_model | half_map_with_model, external_tls |
| TLS step writes a changed T back into the shared object | tls_reused | tls_reused |
| local pass of global-local-global gets get_tls_from_u False | global_local_global_model | both global_local_global cases |

</details>

## Coding agent

| Field | Value |
|-------|-------|
| Model and version | Claude Opus 5.5 (claude-opus-5-5) |
| Harness | Claude Code 2.1.319, VS Code extension |
| Rounds of adversarial review | 3 |
| Reviewer model, version, and harness | Claude Opus 5.5 (claude-opus-5-5), Claude Code 2.1.319 general-purpose subagent; a fresh subagent session per round, given the diff, the problem statement and AGENTS.md / CONTRIBUTING.md, not the writer's reasoning |

<details>
<summary>Final review round</summary>

~~~~~~~~
## Findings

No high or medium findings. Every bug the original cases were written for still fails the new test, in the case written for it (evidence below). There are three low findings.

**Low 1: The global-local-global cases no longer check the TLS options that reach the local pass**
`iotbx/regression/tst_map_model_manager_supplied_tls.py:1137-1146` (`global_local_global_model`) and `:1161-1170` (`global_local_global_half_map`).
- Both cases now stub the sharpening driver. They check only the selections, T, L and the model that reach `_run_group_of_anisotropic_sharpen`.
- The original cases ran the TLS step in the local pass. Through `check_with_model` → `check_tls_run`, they asserted `get_tls_from_u in (None, True)` and the U value of each entry.
- The stub already records `get_tls_from_u`, `get_scale_as_aniso_u` and `replace_aniso_with_tls_equiv` in `run_group_kw_list`, but these two cases never assert them.
- Failure scenario: the local pass of global-local-global sharpening receives `get_tls_from_u = False`. The TLS step then leaves `uu_b_cart_as_u_cart` alone and the supplied TLS is silently ignored.
- Evidence: I ran a mutant of `map_model_manager.py` that sets `kw['get_tls_from_u'] = False` just before the local pass in `_sharpen_overall_local_overall`.
  - The original `global_local_global_model` fails: `AssertionError: False` from `check_tls_run`.
  - The new `global_local_global_model` and `global_local_global_half_map` both pass.
- No past bug matches this, hence low. Commit 02c1e794e9 says these cases "use the stub" but does not say this check was dropped.
- Cheap fix: in both cases, assert `get_tls_from_u is True` and `get_scale_as_aniso_u is True` for every recorded driver call, as `options_passed_on` does.

**Low 2: A section comment does not match the code**
`iotbx/regression/tst_map_model_manager_supplied_tls.py:841-844` says the supplied-TLS cases other than `external_tls` and `other_model_id` "stub the sharpening ... and run the TLS step directly where its result matters". `tls_in_other_frame` (`:1238`) is in that section, has supplied TLS, and stubs nothing; it only calls the TLS step directly.

**Low 3: The fast-job runtime target is missed, and the commit message does not say so**
- The 18 direct and stubbed cases (with the three averaging cases) take 12.1 CPU-s in one process. The design target was under 10 s.
- The two global-local-global cases account for about 8 s, because their overall passes still run.
- Commit 76f827dd34 gives the 12 s and that reason, but does not state that the target was missed or why the overall passes stay unstubbed.
- The other targets are met:
  - `run_tests.py` goes from 25 entries to 6.
  - Total CPU drops from 539 s (longest job 70 s) to 114 s (longest job 26.7 s), which matches the commit message.

## What I checked
- **Mutation testing.** Library imported from origin/master, with a mutated copy of `map_model_manager.py` loaded in its place. Each mutant re-introduces a fix from 12fc98df99, 2c84ae8767, d7f4b2d998 or 1ac7769a32, or one of the extra plumbing/TLS-step mutants listed below; all 20 were killed in the case(s) written for them:
  - first-group TLS: `two_tls_groups`
  - later group wins: `overlap_nearest`
  - growing entry count: `sampling_count`
  - model points re-tested against later masks: `two_tls_groups`, `reversed_order`
  - TLS defined only for groups with grid entries: `empty_mask_group`
  - tie goes to the last group: `tie_first_listed`, `no_model_first_listed`
  - shift sign and unshifted model points: `tls_in_other_frame`
  - overall U kept: several cases
  - averaged overall_scale dropped: `all_tls_entries_updated` and the averaging cases
  - `get_scale_as_aniso_u` / `get_tls_from_u` not passed on: `options_passed_on`
  - conversion not idempotent: `tls_reused` and the global-local-global cases
  - half-map conversion after the global-local-global dispatch: `global_local_global_half_map`
  - TLS model not kept in the working manager: `half_map_with_model`, `global_local_global_half_map`
  - TLS step dropping its default model: `half_map_with_model`
  - TLS step mutating T: `tls_reused`
  - three mutants only a complete run can catch (model_sharpen not passing `model_id_for_tls`, `model_id` dropped between the driver and `_analyze_aniso`, update not called): `other_model_id`
- **Setup guards are not vacuous** (values from the run output):
  - 1 original entry inside no mask
  - 2 entries inside both masks; one has a nearest group that is not the last processed
  - old vs new sampling rule: 50 vs 25 model points
  - lattice: 560 entries, 10 inside both masks
  - 2 exact ties
  - opposite-sign U difference 2.0e-4 against a 2e-6 threshold
- **Synthetic constants match a complete model_sharpen run:** the entry is at (10.832, 17.715, 11.430); `aniso_b_cart` is (170.76, 174.15, 168.69, -2.66, 0.44, 4.46) and `b_iso` is 20.82.
- **Stub fidelity:** recorded options and T/L come from the library's own objects. The model check uses the working manager the driver receives. `uu_before` holds tuples (`adptbx.b_as_u`), so its `==` check is meaningful on real entries too.
- **Test-code correctness:** cache keys are distinct (stub flag included), reversed runs index-match the synthetic sites, the class methods are restored in `finally`, and every run uses a temporary directory. No files are left in the working directory after a full run.
- **Running the file:**
  - each of the 18 fast cases passes alone
  - the whole file with no arguments passes (109 CPU-s)
  - all 25 case names appear in `run_tests.py`, and all 6 jobs pass
- **Conventions:** `libtbx.find_clutter` is clean on both files and no new lines exceed 80 columns. Only `iotbx/regression/tst_map_model_manager_supplied_tls.py` and `iotbx/run_tests.py` change. Commit summaries are scoped and at most 72 columns, with no attribution trailers or tracker keys.
~~~~~~~~

</details>

The commit hashes in the review refer to the branch before its review
fixes were folded into the two commits of this pull request.

- Low 1, fixed: both global-local-global cases assert
  `get_scale_as_aniso_u` and `get_tls_from_u` True for every driver call;
  the reviewer's mutant now fails both.
- Low 2, fixed: section comment reworded.
- Low 3, fixed: the message of the `run_tests.py` commit now says the
  10 s target was missed and why the overall passes stay unstubbed.

One change after the final round, for a test-run failure: the fast group
was listed in `run_tests.py` as 18 case names, which made too long a
directory name under `run_in_unique_dirs` ("File name too long" on the
full regression run). It is now the single argument `fast`.

## Checklist
- [x] Searched open and closed PRs and issues for prior work (none found)
- [x] New or changed tests are registered in the module's `run_tests.py`
- [x] Docstrings and PHIL `.help` strings cover the new or changed behavior
- [x] No new external dependency, or it was agreed with the developers
- [x] No imports of modules outside this repository; `chem_data` and
      `phenix_regression` are used only for data, with a skip when absent
- [x] Interface or breaking changes are called out under Changes (none)
- [x] Agent-written code: the writing agents had `AGENTS.md` and
      `CONTRIBUTING.md` in context, and the reviewing agents had
      `AGENTS.md`
- [x] A human has reviewed the complete diff

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `68ae5b9fb614`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.