# Issue

**Title:** mmtbx.reduce2: Expose the water H placement settings

## Description

## Problem
Since #1291, reduce2 places water H with mmtbx.development.virvel at fixed settings: water H in the input are always placed again, symmetry mates are always included, and the search runs at virvel's defaults. virvel's own program lets users change these; reduce2 did not.

## Changes
- New PHIL scope `water_hydrogens` with virvel's placement settings; the defaults place water H as in #1291. `geometry`, `element`, `stats` and `output` stay out: reduce2's own settings decide geometry and H or D.
- A setting that cannot take effect, or one given as None, is a Sorry (None also in virvel's program).
- The description lists the waters that had one H in the input, through `partial_water_lines`, now shared with virvel's program; its water H lines are worded differently.
- Interface: the new scope and two new functions (`partial_water_lines`, `hd_exchange.water_isotope`); no existing parameter changes. reduce2 version 3.12.0.

## Tests
- New `mmtbx/regression/tst_reduce2_water_h_settings.py` (registered); `tst_water_protonation.py` extended. Each new test fails against master's program.
- Linux (cluster), full suite: 7 failures, the same 7 as master on that machine.

## Coding agent

| Field | Value |
|-------|-------|
| Model and version | Claude Opus 5.5 (claude-opus-5-5) |
| Harness | Claude Code 2.1.311: the session and, for the PHIL design, its workflow subagents |
| Rounds of adversarial review | 4 |
| Reviewer model, version, and harness | Claude Opus 5.5 (claude-opus-5-5), Claude Code 2.1.311 subagent |

<details>
<summary>Final review round</summary>

~~~~~~~~
## Review: reduce2 `water_hydrogens` settings (branch `reduce2_water_h_options`, 2 commits on origin/master)

**Scope.** I reviewed the full diff against the merge base with the current `origin/master` (c9d1d90e63): 7 files, +478/-36. During the review the uncommitted edit (the "(model N)" suffix) was folded into HEAD (now 6b74c5c37f). The tree content is the same as what I reviewed, and there are no untracked files.

**What I ran:**
- `tst_reduce2_water_h_settings.py` passes in 5.4 s.
- `tst_water_protonation.py`, `tst_reduce2_water_h.py` and `tst_reduce2_water_phantoms.py` pass.
- All 49 registered `tst_reduce2*` tests ran. The only failures are the four known master failures (altloc_charge, capping, neutron_ligands, symmetry_optimizer), with the same unrelated errors: nucleic-acid H names, model interpretation, a `bond_list` on None.
- `libtbx.find_clutter` on the changed files reports nothing.
- The only added line over 80 columns is virvel's quoted `max_sweeps` help, which follows that file's style.

**Fixed requirements, all met:**
- The scope is named `water_hydrogens`. It has no `geometry`, `element`, `stats` or `output`.
- reduce2 keeps its own PHIL. `exercise_defaults` checks the search defaults against virvel's. `existing_h` defaults to `reorient`.
- `symmetry` goes through `reduce_hydrogen.contact_symmetry`, so it skips only a missing cell and the 1 1 1 placeholder, not cryo-EM boxes.
- When water H are placed:
  - qmready with approach=place is still refused by validate's place check.
  - remove with add_h_to_water=True is still a Sorry.
- None values are refused by an explicit check in validate().
- virvel's output is unchanged apart from the `max_sweeps` caption and help. I checked this by running master's `_print_partial_waters` (from `git show`) against the branch's on 300 random single-H lists: 0 differences.
- Both test files are self-contained.

**Description report, checked by hand on the test model:**
- The counts are correct for reorient, keep and complete.
- D/DOD and H/D are used under `all_d`.
- A DOD carrying H is listed as HOH.
- Multi-model entries end in "(model N)", as in reduce2's other reports.

**Each validate() message, checked:**
- remove: "cannot be used with approach=remove".
- No water H placed: "requires add_h_to_water=True, or qmready=True with approach=add".
- keep or complete without the input H kept: the message names the settings that keep them for that approach.

### High
None.

### Medium
None.

### Low
1. **The default (reorient) report is not tested.** mmtbx/regression/tst_reduce2_water_h_settings.py:127. `exercise_existing_h` covers only keep and complete. No test asserts what a default run with water H in the input now writes: the "Water H in the input: N (existing_h=reorient)" line and the "placed again" label. Here is what `approach=optimize add_h_to_water=True` gives on the test's `pdb_str`, which I checked by hand:
   ```
   Water H placed: 4 (O-H 0.850 A, with crystal symmetry)
   Water H in the input: 3 (existing_h=reorient)
   1 single-H water placed again:
     HOH A 5
   ```
   Suggested fix: add a reorient run to that test and assert these two lines. Leave out the loop's check that the input H stay in place, because reorient moves them.

2. **reduce2 depends on an id format the placer does not document.** mmtbx/programs/reduce2.py:1454. reduce2 renames the water by splitting the placer's display string (`name, rest = rid.split(' ', 1)`). That string comes from `_water_id` (mmtbx/hydrogens/water_protonation.py:2387). The Returns section of `place_water_hydrogens` describes `residue_id` but not its format. If that format changes, reduce2's report would mislabel waters without any error. Suggested fix: one line in that Returns section, e.g. `residue_id` is "RESNAME chain resseq", followed by " altloc X" for a conformer.

3. **An option no caller uses.** mmtbx/hydrogens/water_protonation.py:2149. `partial_water_lines(..., max_listed=20)` adds a keyword that neither reduce2, virvel nor the tests pass; it replaces the module constant `_MAX_LISTED_WATERS`. AGENTS.md asks for no options the problem does not require. Suggested fix: keep the cap as a module constant in mmtbx/hydrogens/water_protonation.py and drop the parameter. This is optional.

**Summary:** high 0, medium 0, low 3.
~~~~~~~~

</details>

- Low 1: fixed; `exercise_existing_h` also runs the default `existing_h=reorient` and checks its description lines.
- Low 2: left; the tests pin the id format (`rid == "HOH W 1"` in virvel's, the listed DOD and HOH names in reduce2's), so a change to it fails them.
- Low 3: left; the keyword keeps the limit in the signature and the docstring.

## Checklist
- [x] Searched open and closed PRs and issues for prior work (builds on #1291; none found for these settings)
- [x] New or changed tests are registered in the module's `run_tests.py`
- [x] Docstrings and PHIL `.help` strings cover the new or changed behavior
- [x] No new external dependency, or it was agreed with the developers
- [x] No imports of modules outside this repository; `chem_data` and `phenix_regression` are used only for data, with a skip when absent
- [x] Interface or breaking changes are called out under Changes
- [x] Agent-written code: the writing agents had `AGENTS.md` and `CONTRIBUTING.md` in context, and the reviewing agents had `AGENTS.md`
- [x] A human has reviewed the complete diff

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `21ddbc57c256`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.